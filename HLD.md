**High Level Design Document**


#### **Detailed Description of Each Component:**



1. **Upload API**:
    * **Function**: Handles the upload of CSV files from clients. Performs basic validation of the CSV format and content.
    * **Role**: Ensures that the uploaded CSV files are correctly formatted and contain valid data before initiating processing. Generates a unique request ID and enqueues the task for processing.
    * **Implementation**:
        * Receives the CSV file via a POST request.
        * Validates the CSV format and content.
        * Generates a unique request ID.
        * Stores the initial request status ("pending") in the database.
        * Enqueues the task for asynchronous processing in asynchronous task queue.
        * Responds with the unique request ID.
    

2. **Status API**:
    * **Function**: Provides an endpoint for clients to check the processing status of their requests using the request ID.
    * **Role**: Allows clients to monitor the progress and completion status of their image processing requests.
    * **Implementation**:
        * Receives a GET request with the request ID.
        * Queries the database for the status and results of the specified request.
        * Responds with the current status and processed image URLs.

3. **Asynchronous Task Queue**:
    * **Function**: Manages and distributes processing tasks to worker nodes for asynchronous processing.
    * **Role**: Decouples task submission from task processing, enabling scalable and efficient handling of image processing tasks.
    * **Implementation**:
        * Typically implemented using message queue systems like Apache KAFKA.
        * Tasks are enqueued with relevant data (request ID, CSV rows) and processed by worker nodes asynchronously.
4. **Worker Nodes**:
    * **Function**: Retrieve tasks from the queue, process CSV data, and interact with the image processing service.
    * **Role**: Execute the actual processing tasks asynchronously.
    * **Tasks**:
        * Fetch tasks from the queue.
        * Pass CSV rows and send image URLs to the image processing service.
        * Track processing status and update the database.
    * **Implementation**:
        * Fetch tasks from the queue and pass the row data to image processing service.
        * Update the database with processing results and status.
5. **Image Processing Service**:
    * **Function**: Processes images asynchronously based on input from worker nodes.
    * **Role**: Perform the actual image transformations and generate new URLs.
    * **Operation**: External service receiving image URLs, processing them, and sending a callback upon completion.
    * **Existing Implementation**:
        * Receives image processing requests from the worker nodes.
        * Processes the images and generates new URLs.
        * Sends a callback to the webhook handler upon completion.
6. **Webhook Handler**:
    * **Function**: Listens for callbacks from the image processing service when processing is complete.
    * **Role**: Updates the database with processing results and changes request statuses accordingly.
    * **Implementation**:
        * Receives POST requests from the image processing service with processing results.
        * Updates the `Images` table with processed image URLs and status.
        * Checks if all rows for a request are processed and updates the request status to "completed" if applicable.
