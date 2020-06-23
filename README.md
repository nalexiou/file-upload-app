# Uploading files with Multer in Node

After cloning repository, run:  npm install

## Development server

Run `node server.js` for a dev server. Navigate to `http://localhost:8080/`.

To build docker image, run the following command inside application directory

 docker build -t sampleapp:dev .
 
You can run image with the following command:

docker run -d --rm -v {pathToLocalDir}:/uploads -p 8080:8080 sampleapp:dev

