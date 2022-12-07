# AR Cut & Paste

This fork fixes the image scaling issues and introduces a more generic method to paste the image rather than using only photoshop

## Modules

This prototype runs as 2 independent modules:

- **The local server**

  - The interface between the mobile app and Photoshop.
  - It finds the position pointed on screen by the camera using [screenpoint](https://github.com/cyrildiagne/screenpoint)
  - Check out the [/server](/server) folder for instructions on configuring the local server

- **The object detection / background removal service**

  - For now, the salience detection and background removal are delegated to an external service
  - It would be a lot simpler to use something like [DeepLap](https://github.com/shaqian/tflite-react-native) directly within the mobile app. But that hasn't been implemented in this repo yet.

## Usage

### NOTE: THE PASTE ENDPOINT ONLY WORKS ON MACOS DUE TO COMPATIBILITY ISSUES

### 1 - Setup the external salience object detection service

- Follow instruction for the repo here: [BASNet-HTTP wrapper](https://github.com/anojht/basnet-http)

#### Set up your own model service (requires a CUDA GPU)

- As mentioned above, for the time being, you must run the
  BASNet model (Qin & al, CVPR 2019) as an external HTTP service (requires a CUDA GPU)

- You will need the running service URL to configure the local server

- Make sure to check the port configuration if you're running BASNet on the same computer as the local service

### 2 - Configure and run the local server

## Setup

```console
virtualenv -p python3.10 venv
source venv/bin/activate
pip install -r requirements.txt
```

## Run

```console
python main.py \
    --basnet_service_ip="http://X.X.X.X:8080" \
```

**NOTE: This app runs on port 8081 and you can set the port in main.py. Make sure to update the Android app to use the IP address of your laptop so it can connect.**

## Thanks and Acknowledgements

- [Original Repo](https://github.com/cyrildiagne/ar-cutpaste) for serving as an excellent base to improve upon
- [BASNet code](https://github.com/NathanUA/BASNet) for '[_BASNet: Boundary-Aware Salient Object Detection_](http://openaccess.thecvf.com/content_CVPR_2019/html/Qin_BASNet_Boundary-Aware_Salient_Object_Detection_CVPR_2019_paper.html) [code](https://github.com/NathanUA/BASNet)', [Xuebin Qin](https://webdocs.cs.ualberta.ca/~xuebin/), [Zichen Zhang](https://webdocs.cs.ualberta.ca/~zichen2/), [Chenyang Huang](https://chenyangh.com/), [Chao Gao](https://cgao3.github.io/), [Masood Dehghan](https://sites.google.com/view/masoodd) and [Martin Jagersand](https://webdocs.cs.ualberta.ca/~jag/)
- RunwayML for the [Photoshop paste code](https://github.com/runwayml/RunwayML-for-Photoshop/blob/master/host/index.jsx)
- [CoreWeave](https://www.coreweave.com) for hosting the public U^2Net model endpoint on Tesla V100s
