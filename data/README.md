# Data Sources and Access

All files in `raw/` were downloaded from the sources below and translated from Korean to English by our team. Only column names and Korean text labels were translated; every number and code is unchanged. Files are UTF-8 encoded (read with `encoding="utf-8-sig"`).

We use **2025 data only** (quarter codes 20251 to 20254). The raw files include other years, which the notebook filters out.

## Raw data (`raw/`)

| File | Original dataset | Provider | Source |
|---|---|---|---|
| Seoul_Estimated_Sales_by_Dong.csv.zip | 서울시 상권분석서비스(추정매출-행정동) | Seoul Metropolitan Government | https://data.seoul.go.kr/dataList/OA-22175/A/1/datasetView.do |
| Seoul_Stores_by_Dong.csv | 서울시 상권분석서비스(점포-행정동) | Seoul Metropolitan Government | https://data.seoul.go.kr/dataList/OA-22172/S/1/datasetView.do |
| Seoul_Resident_Population_by_Dong.csv | 서울시 상권분석서비스(상주인구-행정동) | Seoul Metropolitan Government | https://data.seoul.go.kr/dataList/OA-22183/S/1/datasetView.do |
| Seoul_Workplace_Population_by_Dong.csv | 서울시 상권분석서비스(직장인구-행정동) | Seoul Metropolitan Government | Seoul Open Data Plaza (search the Korean title) |
| Seoul_Fitness_Center_Licenses.csv | 서울시 체력단련장업 인허가 정보 | Seoul Metropolitan Government | https://data.seoul.go.kr/dataList/OA-16142/A/1/datasetView.do |
| Seoul_Obesity_by_Borough_2025.csv | 서울시 비만도 통계 | Seoul Metropolitan Government (KDCA Community Health Survey) | https://data.seoul.go.kr/dataList/DT201004E0500052001/S/2/datasetView.do |
| Seoul_Dong_Boundaries_ver20250701.geojson | HangJeongDong_ver20250701.geojson (Seoul only) | vuski/admdongkor | https://github.com/vuski/admdongkor (folder `ver20250701`) |
| Seoul_Dong_Area_Centroids.csv | 서울시 상권분석서비스(영역-행정동) | Seoul Metropolitan Government | https://data.seoul.go.kr/dataList/OA-22160/S/1/datasetView.do |
| Seoul_Consumption_by_Dong.csv | 서울시 상권분석서비스(소비-행정동) | Seoul Metropolitan Government | Seoul Open Data Plaza (search the Korean title) |
| Seoul_Commercial_Change_Indicator_by_Dong.csv | 서울시 상권분석서비스(상권변화지표-행정동) | Seoul Metropolitan Government | Seoul Open Data Plaza (search the Korean title) |

The last three files are not used in Session 2. We kept them as possible inputs.

### Notes

- **Boundaries:** the original GeoJSON covers all of Korea. We kept only Seoul (`adm_cd2` starting with `11`) and translated the place names. Geometry is unchanged. Please check the repository's `LICENSE-DATA` for attribution terms.
- **Obesity:** the original table had a 4-row header, which we flattened into one row of English column names. The `Total` row is the Seoul average and is removed before merging.
- **Gym licenses:** addresses and business names were left in Korean, since we place gyms using coordinates, not addresses.
- **Sales file is zipped:** the CSV is 36 MB, over GitHub's 25 MB web upload limit, so we store it as a .zip (15 MB). pandas reads it directly with `pd.read_csv("...csv.zip")`, no need to unzip.
- **Encoding:** the original Seoul files use CP949 (Korean Windows encoding). Our translated versions are UTF-8.

## Processed data (`processed/`)

Created by `notebooks/01_data_integration.ipynb`.

| File | Description |
|---|---|
| dong_features_2025.csv | One row per 행정동 (365 rows): identifiers, the 8 PCA variables (before transformation), and context variables (population, workplace share, obesity) |
| dong_pca_input_standardized.csv | The 8 PCA variables after transformation and standardization (365 × 8). This is the PCA input |

Created by `notebooks/02_pca.ipynb`.
| File | Description |
|---|---|
| dong_pca_scores.csv | PC1–PC4 scores for each 행정동 (365 × 4), created by `02_pca.ipynb` |
| pca_loadings.csv | Loadings of the 8 variables on PC1–PC4 (8 × 4), created by `02_pca.ipynb` |
| pca_explained_variance.csv | Eigenvalue and explained variance ratio for all 8 components, created by `02_pca.ipynb` |

## How to reproduce

1. Clone the repository.
2. Install the requirements: `pip install -r requirements.txt`
3. Open `notebooks/01_data_integration.ipynb` and run all cells. It reads from `data/raw/` and writes to `data/processed/` and `figures/`.
4. Open `notebooks/02_pca.ipynb` and run all cells. It reads from `data/processed/` and writes the PCA outputs and figures.

Run the notebooks from inside the `notebooks/` folder, because file paths are relative to it.
