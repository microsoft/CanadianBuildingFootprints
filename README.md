Introduction
-------------------
This dataset contains 12,663,475 computer generated building footprints in all Canadian provinces and territories. This data is freely available for download and use.

License
-------------------
This data is licensed by Microsoft under the [Open Data Commons Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/)

## FAQ
#### What the data include:
12,663,475 building footprint polygon geometries from all Canadian provinces and territories in GeoJSON format.

#### What is the GeoJson format?
GeoJSON is a format for encoding a variety of geographic data structures. 
For Intensive Documentation and Tutorials, Refer to [GeoJson Blog](http://geojson.org/)

#### Creation Details:
The building extraction is done in two stages:
1.	Semantic Segmentation – Recognizing building pixels on the aerial image using DNNs
2.	Polygonization – Converting building pixel blobs into polygons
### Semantic Segmentation
![](/images/segmentation.png)


#### DNN architecture
The network foundation is ResNet34 which can be found [here](https://github.com/Microsoft/CNTK/blob/master/PretrainedModels/Image.md#resnet). In order to produce pixel prediction output, we have appended RefineNet upsampling layers described in this [paper](https://arxiv.org/abs/1611.06612).
The model is fully-convolutional, meaning that the model can be applied on an image of any size (constrained by GPU memory, 4096x4096 in our case).

#### Training details
The training set consists of 3 million labeled images. Majority of the satellite images cover diverse residential areas in Canada. For the sake of good set representation, we have enriched the set with samples from various areas covering mountains, glaciers, forests, beaches, coasts, etc.
Images in the set are of 256x256 pixel size with 1 ft/pixel resolution.
The training is done with CNTK toolkit using 32 GPUs.

### Polygonization
![](/images/polygonization.PNG)

#### Method description
We developed a method that approximates the prediction pixels into polygons making decisions based on the whole prediction feature space. This is very different from standard approaches, e.g. Douglas-Peucker algorithm, which are greedy in nature. The method tries to impose some of a priori building properties, which is, at the moment, manually defined and automatically tuned.

#### Metrics
Building matching metrics on our evaluation set:

| Metric | Value |
| --- | :---: |
| Precision | 97.5% |
| Recall | 74.5% |

False positive ratio across the board is 7.5%, or one false positive per 4 mi<sup>2</sup> area. We have noticed particular extraction problems on agricultural fields that we'll try to fix in the next iteration.

We track various metrics to measure the quality of the output:
1. Intersection over Union – This is the standard metric measuring the overlap quality against the labels
2. Shape distance – With this metric we measure the polygon outline similarity
3. Dominant angle rotation error – This measures the polygon rotation deviation

![](/images/bldgmetrics.JPG)

On our evaluation set contains ~45k building. The metrics on the set are:
- IoU is 0.76, Shape distance is 0.43, Average rotation error is 3.7 degrees

#### Data Vintage
The vintage of the footprints depends on the vintage of the underlying imagery. Because Bing Imagery is a composite of multiple sources it is difficult to know the exact dates for individual pieces of data.

#### How good are the data?
Our metrics show that in the vast majority of cases the quality is at least as good as data hand digitized buildings in OpenStreetMap. It is not perfect, and false positives exist, but most areas look awesome. 

### What is the coordinate reference system?
EPSG: 4326

#### Will there be more data coming for other geographies?
Maybe. This is a work in progress.

#### Why is the data being released?
Microsoft has a continued interest in supporting the open data ecosystem.

#### Should we import the data into OpenStreetMap?
Maybe. Never overwrite the hard work of other contributors or blindly import data into OSM without first checking the local quality. While our metrics show that this data meets or exceeds the quality of hand drawn building footprints, the data does vary in quality from place to place, between rural and urban, mountains and plains, and so on. Inspect quality locally and discuss an import plan with the community. Always follow the [OSM import community guidelines](https://wiki.openstreetmap.org/wiki/Import/Guidelines).

| State         | Number of Buildings  | Unzipped MB |
| ------------- |:-------------:| -----:|
| [Alberta](https://usbuildingdata.blob.core.windows.net/canadian-buildings/Alberta.zip)|1,937,284|453|
| [British Columbia](https://usbuildingdata.blob.core.windows.net/canadian-buildings/BritishColumbia.zip)|1,488,776|349|
| [Manitoba](https://usbuildingdata.blob.core.windows.net/canadian-buildings/Manitoba.zip)|723,502|175|
| [New Brunswick](https://usbuildingdata.blob.core.windows.net/canadian-buildings/NewBrunswick.zip)|351,640|74|
| [Newfoundland and Labrador](https://usbuildingdata.blob.core.windows.net/canadian-buildings/NewfoundlandAndLabrador.zip)|265,376|54|
| [Northwest Territories](https://usbuildingdata.blob.core.windows.net/canadian-buildings/NorthwestTerritories.zip)|17,326|5|
| [Nova Scotia](https://usbuildingdata.blob.core.windows.net/canadian-buildings/NovaScotia.zip)|400,826|81|
| [Nunavut](https://usbuildingdata.blob.core.windows.net/canadian-buildings/Nunavut.zip)|4,191|1|
| [Ontario](https://usbuildingdata.blob.core.windows.net/canadian-buildings/Ontario.zip)|3,832,076|826|
| [Prince Edward Island](https://usbuildingdata.blob.core.windows.net/canadian-buildings/PrinceEdwardIsland.zip)|76,606|16|
| [Quebec](https://usbuildingdata.blob.core.windows.net/canadian-buildings/Quebec.zip)|2,535,293|527|
| [Saskatchewan](https://usbuildingdata.blob.core.windows.net/canadian-buildings/Saskatchewan.zip)|1,013,250|281|
| [Yukon](https://usbuildingdata.blob.core.windows.net/canadian-buildings/YukonTerritory.zip)|17,329|4|

<br>
<br>

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

# Legal Notices

Microsoft and any contributors grant you a license to the Microsoft documentation 
in this repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode),
see the [LICENSE](LICENSE) file, and grant you a license to any code in the repository under the [MIT License](https://opensource.org/licenses/MIT), see the
[LICENSE-CODE](LICENSE-CODE) file.

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at http://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/

Microsoft and any contributors reserve all others rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.
