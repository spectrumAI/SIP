# Land cover land use classification from Modis images using the SIP software

## Background

Land cover and land use (LCLU) maps are critical in various applications. The Moderate Resolution Imaging Spectroradiometer (MODIS) sensor aboard the Terra (originally known as EOS AM-1) and Aqua (originally known as EOS PM-1) satellites provides an essential way for large-scale high-frequency LCLU monitoring, due to its wide coverage and high temporal resolution. Neverthless, efficient LCLU mapping from MODIS images are challenging due to (1) the large data volume of MODIS data, and (2) the variability and ambiguity in the spectral-temporal signatures of different LCLU classes. Leveraging GPU computation, the deep learning approach provides an efficient way to deal with big MODIS data. Moreover, deep learning approaches can efficiently capture the subtle and weak signature information of different LCLU classes in a data-driven manner. The investigation on advanced deep neural network approaches for large-scale fast LCLU mapping therefore is an important research issue.   

## Overall Objective

  * Design a data-processing pipeline that can efficiently transform MODIS data into readable format for deep neural networks. The preprocessing steps includes downloading, unzipping, band extraction, reprojection, mosaicing, image enhancement, and splitting into training samples. 

  * Explore the importance of bands in MODIS 250m land products for LCLU classification and identify the "best" band combinations. 

  * Compare different deep learning models to identify the "best" classifiers.

  * Using the identified band combinations and classifiers to perform large-scale LCLU mapping.  
 
## Tutorials

* [Modis 250m land products pre-processing](./modis_250m_preprocessing.md)
* [Modis 250m classification using NDVI time series](./modis_250m_cls_ndvi_time_series.md)
<!--* [Modis 250m land products band importance exploration](./modis_250m_band_importance.md)
* [Classifier comparison for LCLU mapping from Modis 250m products](./modis_250m_classifier_comparison.md)
* [Large scale LCLU mapping from Modis 250m products](./modis_250m_large_scale_mapping.md) -->


## References

1. Latifovic, R., Pouliot, D., & Olthof, I. (2017). Circa 2010 land cover of Canada: Local optimization methodology and product development. Remote Sensing, 9(11), 1098.

2. Guindon, L., Bernier, P. Y., Beaudoin, A., Pouliot, D., Villemaire, P., Hall, R. J., ... & St-Amant, R. (2014). Annual mapping of large forest disturbances across Canada’s forests using 250 m MODIS imagery from 2000 to 2011. Canadian Journal of Forest Research, 44(12), 1545-1554.

3. Latifovic, R., Zhu, Z. L., Cihlar, J., Giri, C., & Olthof, I. (2004). Land cover mapping of North and Central America—global land cover 2000. Remote sensing of environment, 89(1), 116-127.

4. Colditz, R. R., Saldaña, G. L., Maeda, P., Espinoza, J. A., Tovar, C. M., Hernández, A. V., ... & Ressl, R. (2012). Generation and analysis of the 2005 land cover map for Mexico using 250 m MODIS data. Remote Sensing of Environment, 123, 541-552.

5. Zhou, F., Zhang, A., & Townley-Smith, L. (2013). A data mining approach for evaluation of optimal time-series of MODIS data for land cover mapping at a regional level. ISPRS Journal of Photogrammetry and Remote Sensing, 84, 114-129.

6. Zhan, X., Sohlberg, R. A., Townshend, J. R. G., DiMiceli, C., Carroll, M. L., Eastman, J. C., ... & DeFries, R. S. (2002). Detection of land cover changes using MODIS 250 m data. Remote Sensing of Environment, 83(1-2), 336-350.

7. Talukdar, S., Singha, P., Mahato, S., Pal, S., Liou, Y. A., & Rahman, A. (2020). Land-use land-cover classification by machine learning classifiers for satellite observations—A review. Remote Sensing, 12(7), 1135.

8. Wang, J., Zhao, Y., Li, C., Yu, L., Liu, D., & Gong, P. (2015). Mapping global land cover in 2001 and 2010 with spatial-temporal consistency at 250 m resolution. ISPRS Journal of Photogrammetry and Remote Sensing, 103, 38-47.

9. Gómez, C., White, J. C., & Wulder, M. A. (2016). Optical remotely sensed time series data for land cover classification: A review. ISPRS Journal of Photogrammetry and Remote Sensing, 116, 55-72.

10. Wulder, M. A., Coops, N. C., Roy, D. P., White, J. C., & Hermosilla, T. (2018). Land cover 2.0. International Journal of Remote Sensing, 39(12), 4254-4284.

11. Friedl, M. A., McIver, D. K., Hodges, J. C., Zhang, X. Y., Muchoney, D., Strahler, A. H., ... & Schaaf, C. (2002). Global land cover mapping from MODIS: algorithms and early results. Remote sensing of Environment, 83(1-2), 287-302. 


