This is a sample app created for Irembo takeaway questions
### Build using
```docker build -t <your username>/irembonode .```
### Run using
```docker run -p 3000:3000 -d <your username>/irembonode:<helm chart verison>```
### Push to hub 
``` docker push <your username>/irembonode:<helm chart verison> ```
Since this image is used by the helm chart we tag it with a corresponding version