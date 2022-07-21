# Radatron Dataset Documentation
Radatron: Accurate Detecton using Multi-resolution Cascaded MIMO Radar, ECCV 2022.

## Download Dataset
Please download the pre-processed dataset from [here](https://uofi.box.com/v/radatrondataset) and extract it.

## Dataset Description
Between February 2021 to March 2021, we traversed through the twin-cities of Urbana-Champaign in the state of Illinois, USA, three times per week on average using our Radatron dataset collection platform mounted on our car. With a total driving distance spanning over 500 miles and experiments spanning 12 days, our data collection platform consisting of a TI-MMWCAS cascaded MIMO radar and a ZED stereo-camera, provided us with close to 500,000 frames of radar and stereo-camera data. After refining the data and filtering out the empty-frames with no objects, our final dataset consists of 152,000 frames translating to 4.2 hours of driving. 
The data was collected in diverse scenarios including campus roads, our local urban streets, and downtown areas of Champaign and Urbana. We drove through these areas via a variety of different roots to enhance the robustness of the collected dataset. 

## Sensor Suite
The Radatron dataset collection platform was equipped with the following sensors:

- Camera:
    + 1 x ZED Stereo Camera, 1920x1080x3, 88° HFoV, 57° VFoV, 12cm baseline, 20m range.
- Radar:
    + 1 x TI-MMWCAS Cascaded MIMO Radar, 50m range, 5cm range resolution, azimuth resolution 1.2°, elevation resolution 18°.

## Data Format
For distribution we have divided up the datasets into different days. The complete dataset has been packaged as a zip archive for downloading. 

### Images
All images are stored as lossless-compressed PNG files. The files are structured as `<day>/<RGB>/RGB_<day_num>_<exp_num>_<file_num>_<frm_num>.png`, where `<day_num>` corresponds to the day the data was collected (e.g. ‘day1’), `<exp_num>` , `<file_num>`, `<file_num>` correspond to the experiment number (e.g. ‘exp1’) for that day, file number (e.g. file20) for that experiment, and frame number for that file respectively. 

### Heatmaps
Heatmaps are stored in .mat files using a similar naming convention as images i.e. `<day>/heatmap_<h_type>/radar_<day_num>_<exp_num>_<file_num>_<frm_num>.mat`. There are different variants of the radar heatmaps, each stored in a separate folder but with the same file name. The distribution is as follows:
-	`heatmap_HighRes` corresponds to the high-resolution radar heatmap obtained after processing the full emulated uniform 86x1 virtual antenna array of the cascaded MIMO radar followed by motion compensation using Radatron’s motion-induced distortion compensation algorithm.
-	`heatmap_LowRes` corresponds to the low-resolution radar heatmap obtained by processing only the non-uniform 16x1 virtual antenna array using 1 TX and all RX antennas elements.
-	`heatmap_1chip` corresponds to the radar heatmap data processed to have equivalent resolution as the radar used in prior work (refer to the paper for more explanation).
-	`heatmap_NoFix` corresponds to the same high-resolution radar heatmap data as the ‘heatmap_HighRes’ but without Radatron’s motion induced distortion compensation algorithm. 

Each heatmap file is a .mat file consisting of one variable ‘heatmap’ which is the 2D radar heatmap in the polar coordinate system. The dimensions of the heatmap are 512x192 with 512 values for range (0 to 25.55m with a resolution of 0.05m) and 192 values for azimuth angle (0° to 179° with a resolution of 0.93°).


### Ground Truth
Ground truth files are stored in .mat files using the same naming convention i.e. `<day>/GT/bb_<day_num>_<exp_num>_<file_num>_<frm_num>.mat`. Each file consists of two variables `bb_2d` and `bb_clwa`. 
-	`bb_2d` consists of the coordinates of the 2D boxes in the XYXY format and has dimensions of Nx4x2 where ‘N’ corresponds to the number of boxes in the heatmap and there are 4 points of each box with the values for the x and y cooridnates. 
-	`bb_clwa` consists of the coordinates of the 2D boxes in the XYHWA format and has dimensions of Nx5 where ‘N’ again corresponds to the number of boxes in the heatmap and there are 5 values for each box. The 5 values correspond to the x and y coordinates of the center of the box, the height of the box, the width of the box, and the angle of the box w.r.t the vertical axis (a car straight w.r.t the ego-vehicle has an angle of 0°). 

All units are in meters.
