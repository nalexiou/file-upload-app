# Uploading files with Multer in Node

To run app locally, execute the following commands where repository was cloned:

`cd file-upload-app`
`mkdir uploads`
`npm install`

## Development server

Run `node server.js` for a dev server. Navigate to `http://localhost:8080/`.

To build docker image, run the following commands inside application directory

`docker build -t sampleapp:dev .`
 
You can run image to start up dev server with the following command:

`docker run -d --rm -v {pathToLocalDir}:/uploads -p 8080:8080 sampleapp:dev`

