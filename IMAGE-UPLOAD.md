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

* The URL to the qimg API
  (again, in development with `ionic serve`, you will need a proxy, while in production you want to use the real URL)
* The qimg API authentication token

Apply the following changes to your app:

1. Add a new proxy to the qimg API in `ionic.config.json`:

   ```json
   {
     "path": "/qimg-api-proxy",
     "proxyUrl": "https://comem-qimg.herokuapp.com/api"
   }
   ```
2. Add two new `qimgUrl` and `qimgSecret` properties to `config/development.json` using the new proxy:

   ```json
   "qimgUrl": "/qimg-api-proxy",
   "qimgSecret": "..."
   ```
3. Add two new `qimgUrl` and `qimgSecret` properties to `config/production.json`, this time using the real qimg API URL:

   ```json
   "qimgUrl": "https://comem-qimg.herokuapp.com/api",
   "qimgSecret": "..."
   ```
4. Update `constants.js` to define two new `qimgUrl` and `qimgSecret` constants with placeholders:

   ```js
   .constant('qimgUrl', '@qimgUrl@')
   .constant('qimgSecret', '@qimgSecret@')
   ```
5. Update `gulpfile.js` to replace your new `qimgUrl` and `qimgSecret` placeholders:

   ```js
   .pipe(replace(/@qimgUrl@/g, config.qimgUrl))
   .pipe(replace(/@qimgSecret@/g, config.qimgSecret))
   ```
6. Run `gulp config-development` or `gulp config-production` to apply the new configuration

After doing this, you `www/js/constants.js` file should look like this:

```js
angular.module('citizen-engagement')
  .constant('apiUrl', '...')
  .constant('mapboxSecret', '...')
  .constant('qimgUrl', '...')
  .constant('qimgSecret', '...')
;
```



## Implementation

Here's an example of a controller that will:

* Allow the user to take a picture
* Allow the user to create an issue:
  * Upload the image to the qimg API to obtain an image URL
  * Create the issue with the new image URL

```js
angular.module('citizen-engagement').controller('NewIssueCtrl', function(apiUrl, CameraService, $http, $ionicPopup, $log, $q, qimgSecret, qimgUrl) {
  var newIssueCtrl = this;

  newIssueCtrl.issue = {};

  newIssueCtrl.takePicture = function() {
    if (!CameraService.isSupported()) {
      return $ionicPopup.alert({
        title: 'Not supported',
        template: 'You cannot use the camera on this platform'
      });
    }

    CameraService.getPicture({ quality: 50 }).then(function(result) {
      $log.debug('Picture taken!');
      newIssueCtrl.pictureData = result;
    }).catch(function(err) {
      $log.error('Could not get picture because: ' + err.message);
    });
  };

  newIssueCtrl.createIssue = function() {
    // First upload the image, then create the issue using the image URL provided by the qimg API
    return postImage().then(postIssue);
  };

  function postImage() {
    if (!newIssueCtrl.pictureData) {
      // If no image was taken, return a promise resolved with "null"
      return $q.when(null);
    }

    // Upload the image to the qimg API
    return $http({
      method: 'POST',
      url: qimgUrl + '/images',
      headers: {
        Authorization: 'Bearer ' + qimgSecret
      },
      data: {
        data: newIssueCtrl.pictureData
      }
    });
  }

  function postIssue(imageRes) {

    // Use the image URL from the qimg API response (if any)
    if (imageRes) {
      newIssueCtrl.issue.imageUrl = imageRes.data.url;
    }

    // Create the issue
    return $http({
      method: 'POST',
      url: apiUrl + '/issues',
      data: newIssueCtrl.issue
    });
  }
});
```



## Authentication

You are going to have to authenticate to the qimg API which is separate from the citizen engagement API.

In the beginning, we added an `AuthInterceptor` to automatically set the `Authorization` header on all our requests.
Be sure to **update** this interceptor, otherwise it will overwrite the Authorization header from the `postImage()` call:

```js
angular.module('citizen-engagement').factory('AuthInterceptor', function(AuthService) {
  return {

    // The request function will be called before all requests.
    // In it, you can modify the request configuration object.
    request: function(config) {

      // If the user is logged in, add the Authorization header (unless it's already there)
      if (AuthService.authToken && !config.headers.Authorization) {
        config.headers.Authorization = 'Bearer ' + AuthService.authToken;
      }

      return config;
    }
  };
});
```



[qimg]: https://mediacomem.github.io/comem-qimg/
