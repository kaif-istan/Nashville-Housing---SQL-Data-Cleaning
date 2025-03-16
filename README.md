# Nashville Housing Data Cleaning 🏡🔍  

## Overview  
This project focuses on cleaning and transforming the **Nashville Housing Dataset** using SQL. The dataset contains information on property sales, including addresses, dates, and zoning details. The goal is to preprocess the data, remove inconsistencies, and ensure it is structured for analysis.  

## Dataset  
- **Source:** [Kaggle - Nashville Housing Dataset](https://www.kaggle.com/code/bvanntruong/nashville-housing-sql-data-cleaning)  
- **Contains:** Property addresses, sale dates, zoning information, owner details, and more.  

## Objectives  
✅ Standardize date formats  
✅ Populate missing property addresses  
✅ Break out full addresses into separate columns  
✅ Remove duplicates and unnecessary columns  
✅ Format and clean text fields  

## SQL Cleaning Steps  
### 1. Standardizing Date Formats  
```sql
UPDATE NashvilleHousing  
SET SaleDate = CONVERT(Date, SaleDate);
```  

### 2. Handling Missing Addresses  
```sql
UPDATE NashvilleHousing  
SET PropertyAddress = (SELECT TOP 1 PropertyAddress  
                      FROM NashvilleHousing AS NH  
                      WHERE NH.ParcelID = NashvilleHousing.ParcelID  
                      AND NH.PropertyAddress IS NOT NULL)  
WHERE PropertyAddress IS NULL;
```  

### 3. Splitting Full Address into Separate Columns  
```sql
ALTER TABLE NashvilleHousing  
ADD Address NVARCHAR(255), City NVARCHAR(255);

UPDATE NashvilleHousing  
SET Address = SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) - 1),
    City = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1, LEN(PropertyAddress));
```  

### 4. Removing Duplicates  
```sql
WITH CTE AS (  
    SELECT *,  
           ROW_NUMBER() OVER (PARTITION BY ParcelID, SaleDate, SalePrice ORDER BY UniqueID) AS RowNum  
    FROM NashvilleHousing  
)  
DELETE FROM CTE WHERE RowNum > 1;
```  

### 5. Dropping Unnecessary Columns  
```sql
ALTER TABLE NashvilleHousing  
DROP COLUMN OwnerAddress, TaxDistrict;
```  

## Technologies Used  
🔹 **SQL (Microsoft SQL Server)**  
🔹 **Kaggle Datasets**  

## Results  
✔ Improved dataset consistency and accuracy  
✔ Enhanced usability for further analysis and visualization  
✔ Ready for predictive modeling and reporting  

## Author  
👤 **Md Kaif**  
📧 Contact: kaif85077@gmail.com  
📂 LinkedIn: [kaif85077](https://www.linkedin.com/in/kaif85077/)  
