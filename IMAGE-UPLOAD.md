# Image Upload

You can use any image upload service you like.
The [qimg API][qimg] is a limited service provided for this course.



## Limitations

* Images cannot be larger than 2MB
* Each user has a quota: a maximum number of images they can upload

  The quota is 10 by default.
  When you start uploading more than 10 images, older images will be deleted.



## Configuration

You should have received a qimg authentication token that grants you access to the qimg API.

There are two new configuration properties your app needs to have:

* The URL to the qimg API.
* The qimg API authentication token.

You should add those to your `src/app/config.ts` file:

```ts
export const config = {
  apiUrl: 'https://comem-citizen-engagement.herokuapp.com/api',
  // TODO: add the qimg URL and secret token to the configuration
  qimgUrl: 'https://comem-qimg.herokuapp.com/api',
  qimgSecret: 'changeme'
}
```

Do not forget to also update `src/app/config.sample.ts` to provide appropriate examples for other developers who might work on the project.



## Model

Here's model you might need.
Save it, for example to `src/models/qimg-image.ts`.
It represents the response from the qimg API when creating an image:

```ts
export class QimgImage {
  id: string;
  size: number;
  url: string;
  createdAt: string;
}
```



## Implementation



### Create a 

```bash
$> ionic generate provider Picture
```

Here's an example of code in a sample `ExamplePage` component that will:

* Take a picture.
* Upload the picture data to the qimg API to obtain an image URL.
* Create an issue in the Citizen Engagement API with the image URL.

```ts
// Other imports...
import { HttpClient } from '@angular/common/http';
import { Camera, CameraOptions } from '@ionic-native/camera';
import { Observable } from 'rxjs/Observable';
import { switchMap } from 'rxjs/operators';

import config from '../../app/config';
import { Issue } from '../../models/issue';
import { QimgImage } from '../../models/qimg-image';

// ...
export class ExamplePage {
  issue: Issue;
  pictureData: string;
  pictureUrl: string;

  constructor(
    // Other constructor parameters...
    private camera: Camera,
    private httpClient: HttpClient
  ) {
    // ...
  }

  takeAndUploadPicture() {
    this.takePicture().pipe(switchMap(() => this.uploadPicture())).subscribe(image => {
      if (image) {
        console.log(`Successfully uploaded image to qimg API`);
        this.pictureUrl = image.url;
      }
    }, err => {
      console.warn('Could not take or upload picture', err);
    });
  }

  private takePicture(): Observable<string> {
    this.pictureData = undefined;

    const options: CameraOptions = {
      quality: 100,
      destinationType: this.camera.DestinationType.DATA_URL,
      encodingType: this.camera.EncodingType.JPEG,
      mediaType: this.camera.MediaType.PICTURE
    };

    const promise = this.camera.getPicture(options).then(pictureData => {
      this.pictureData = pictureData;
      return pictureData;
    });

    return Observable.fromPromise(promise);
  }

  private uploadPicture(): Observable<QimgImage> {
    if (!this.pictureData) {
      return Observable.of(undefined);
    }

    return this.httpClient.post<QimgImage>(`${config.qimgUrl}/images`, { data: this.pictureData }, {
      headers: {
        Authorization: `Bearer ${config.qimgSecret}`
      }
    });
  }
}
```



[qimg]: https://mediacomem.github.io/comem-qimg/
