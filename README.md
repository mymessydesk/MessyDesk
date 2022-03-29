# MessyDesk
MessyDesk is a tool with graphical user interface for collecting, organising and processing all kind of GLAM staff. MessyDesk is a graph platform where you can build network of information based on your needs. Create a network of data where every item, media file and relationship is important and documentable.

## Technical viewpoint
MessyDesk is based on graph database, event queue and microservices. 

## Services
Services are either external APIs or local containers (with API). Service can be for example a language detection service, service for extracting images from PDF or optical character rekognition service. Techinally service can be a wrapper for a Python program, for example, or it can call external API (like Google Vision). 

Service must implement MD-service API.

### MessyDesk Service API
Service API is really simple, thera are only two required endpoints. The first one is **/info**, that tells the name of the service, version, what kind of data it accepts, what it returns and what options can be given to it.

The second one is **/process** endpoint, that is used when service is required to do some work. Note that this endpoint is called by event queue.

##### GET /info
- **name**: {String} // name of the service
- **version**: {String} // version of the service
- **model**: {String} // for machine learnings apps. What model was used for training
- **input**: {Array} // main type for input: 'documents' (like PDF), '3D', 'sounds', 'videos', 'images', 'datarows' (rows in internal database), 'datafiles' (like csv)
- **file_extensions**: {Array} //  for example: ['jpg', 'jpeg', 'png']
- **output**: {Object} // defines where the result of processing is saved.
Service can produce files or data or both. Depending the type of the service, an output is produced per request (for example image conversion) or once per group of requests (like some king of summary)
- **options**: {Object} // options includes all settings of the service that user can interact with.

##### POST /process
- **items**: {Array} // list of items for processing

### Registering service
In order to use service, MessyDesk must know where it is. This is done by registering service with **/register** endpoint. Registering simply means that MD receives the url where new service is located (for example http://localhost:8100). After this, MD asks more information about service from its **/info** endpoint on that URL so that service can be shown in the user interface.

##### POST /register
- payload: {url: [URL]} // register service in url.

### Calling service
Calling a service is simple. Basically one ask from /info endpoint what options service has and then post request with options to /process endpoint.




