# Thakur-Dhakal   MY first spatial analysis 
## pandas to plot points 
import numpy as np 
import geopandas as gpd
from osgeo import gdal
import descartes
from shapely.geometry import Point, Polygon
import matplotlib.pyplot as plt
%matplotlib inline
# register all of the drivers
gdal.AllRegister()
# Open raster data 
df=gdal.Open(f"D:/Qgis doc/Hwasung point/PM_raster.tif")
# Dimensions 
# df.RasterXSize,df.RasterYSize, df.GetProjection(),df.RasterCount
band = df.GetRasterBand(1)
#  find raster top left, x-pix, top left y, y_pix,roation angle, pix_resolution 5 properties
transform = df.GetGeoTransform()
# change image to array

data = band.ReadAsArray()
# set no data value as -9999
band.SetNoDataValue(-9999)
# gdal.Translate('hi.asci',band,format='asci',projWin=[transform[0],transform[1],transform[2],transform[4]])
#let's save the file in asc file 
# gdal_translate "D:/Qgis doc/Hwasung point/PM_raster.tif" -of AAIGrid "existingascraster.asc"

x=[127.8065531,127.8165531,127.81665531,127.8265531]
y=[37.63887227,37.64887227,37.64987227,37.65887227]
color=['red','black','blue','green']
Points=[Point(xy) for xy in zip(x,y)]
fig,ax=plt.subplots()
fig,[ax,ax1]=plt.subplots(2)  # for fig spilting
world = gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))
# ax = world[world.continent == 'Asia'].plot(ax=ax,color='white', edgecolor='black')
Korea = world[world.name == 'South Korea'].plot(ax=ax,color='white', edgecolor='black')
crs={'init':'epsg:4326'}

Geo_df=gpd.GeoDataFrame(crs=crs,geometry=Points)
Geo_df.plot(ax=ax,color=color)



