/*
Cleaning Data in SQL Queries
	Goal: Make data more useable, friendly and standardized 
*/


SELECT *
FROM PortfolioProjects.dbo.NashvilleHousing;

--------------------------------------------------------------------------------------------------------------------------

-- Standardize Date Format

SELECT SaleDate
FROM PortfolioProjects.dbo.NashvilleHousing

ALTER TABLE NashvilleHousing
ALTER COLUMN SaleDate date;

 --------------------------------------------------------------------------------------------------------------------------

-- Populate Property Address data
  -- NULL values can be populated using commom parcel IDs

SELECT *
FROM PortfolioProjects.dbo.NashvilleHousing
WHERE PropertyAddress IS NULL

SELECT a.ParcelID, a.PropertyAddress, b.ParcelID, b.PropertyAddress, ISNULL(a.PropertyAddress, b.PropertyAddress)
FROM PortfolioProjects.dbo.NashvilleHousing a
JOIN PortfolioProjects.dbo.NashvilleHousing b
	ON a.ParcelID = b.ParcelID                     
	AND a.[UniqueID ] <> b.[UniqueID ]
WHERE a.PropertyAddress IS NULL;


UPDATE a
SET PropertyAddress = ISNULL(a.PropertyAddress, b.PropertyAddress) -- ISNULL checks for NULL values and populates them with second input
FROM PortfolioProjects.dbo.NashvilleHousing a
JOIN PortfolioProjects.dbo.NashvilleHousing b
	ON a.ParcelID = b.ParcelID
	AND a.[UniqueID ] <> b.[UniqueID ]
WHERE a.PropertyAddress IS NULL;

--------------------------------------------------------------------------------------------------------------------------

-- Breaking out `PropertyAddress` into Individual Columns (Address, City) Delimiter = ','

SELECT PropertyAddress
FROM PortfolioProjects.dbo.NashvilleHousing;

SELECT
SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1 ) as Address -- String Select from 1 -> (comma -1)
, SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) +1 , LEN(PropertyAddress)) as Address --String Select from (Comma +1) -> End of string
FROM PortfolioProjects.dbo.NashvilleHousing;


-- Extract Address -- 
ALTER TABLE NashvilleHousing
ADD PropertySplitAddress Nvarchar(255);

UPDATE NashvilleHousing
SET PropertySplitAddress = SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1 )


-- Extract City -- 
ALTER TABLE NashvilleHousing
ADD PropertySplitCity Nvarchar(255);

UPDATE NashvilleHousing
SET PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) +1 , LEN(PropertyAddress))

-- Check work -- 
SELECT *
FROM PortfolioProjects.dbo.NashvilleHousing


-- Breaking out `OwnerAddress` into Individual Columns (Address, City,State) Delimiter = ',' Using PARSENAME
SELECT OwnerAddress
FROM PortfolioProjects.dbo.NashvilleHousing;


SELECT
PARSENAME(REPLACE(OwnerAddress, ',', '.' ),3) -- PareseName looks for '.', so we need to repalce our ',' delimiters with '.' --
,PARSENAME(REPLACE(OwnerAddress, ',', '.' ),2)
,PARSENAME(REPLACE(OwnerAddress, ',', '.' ),1)
FROM PortfolioProjects.dbo.NashvilleHousing


--Update Tables -- 

-- Extract Address --
ALTER TABLE NashvilleHousing
ADD OwnerSplitAddress Nvarchar(255);

UPDATE NashvilleHousing
SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress, ',', '.' ),3)


--Extract City-- 
ALTER TABLE NashvilleHousing
ADD OwnerSplitCity Nvarchar(255);

UPDATE NashvilleHousing
SET OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress, ',', '.' ),2)


--Extract State -- 
ALTER TABLE NashvilleHousing
ADD OwnerSplitState Nvarchar(255);

UPDATE NashvilleHousing
SET OwnerSplitState =  PARSENAME(REPLACE(OwnerAddress, ',', '.' ),1);


--------------------------------------------------------------------------------------------------------------------------


-- Change Y and N to Yes and No in "Sold as Vacant" field

SELECT DISTINCT(SoldAsVacant), COUNT(SoldAsVacant)
FROM PortfolioProjects.dbo.NashvilleHousing
GROUP BY SoldAsVacant
ORDER BY 2


SELECT SoldAsVacant
, CASE WHEN SoldAsVacant = 'Y' THEN 'Yes'
	   WHEN SoldAsVacant = 'N' THEN 'No'
	   ELSE SoldAsVacant
	   END
FROM PortfolioProjects.dbo.NashvilleHousing;

UPDATE NashvilleHousing
SET SoldAsVacant = CASE
       WHEN SoldAsVacant = 'Y' THEN 'Yes'
	   WHEN SoldAsVacant = 'N' THEN 'No'
	   ELSE SoldAsVacant
	   END

-----------------------------------------------------------------------------------------------------------------------------------------------------------

-- Remove Duplicates [Not an all-the-time practice]


WITH RowNumCTE AS( -- Creates a temp-table
SELECT *,
	ROW_NUMBER() OVER (
	PARTITION BY ParcelID, 
				 PropertyAddress,
				 SalePrice,
				 SaleDate,
				 LegalReference
				 ORDER BY
					UniqueID
					) row_num -- identifies rows which contain identical data in each of these columns 

FROM PortfolioProjects.dbo.NashvilleHousing
)
DELETE
FROM RowNumCTE
WHERE row_num > 1


---------------------------------------------------------------------------------------------------------

-- Delete Unused Columns [Again, don't usually do this to raw data]
	-- OwnerAddress, PropertyAddress, & TaxDistrict

SELECT *
FROM PortfolioProjects.dbo.NashvilleHousing

ALTER TABLE PortfolioProjects.dbo.NashvilleHousing
DROP COLUMN OwnerAddress, PropertyAddress, TaxDistrict

-----------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------

