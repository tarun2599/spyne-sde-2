### **API Documentation**


#### **1. Upload API**



* **Endpoint**: `/upload`
* **Method**: `POST`
* **Description**: Receives CSV files, validates the data, generates a unique request ID, and enqueues tasks for image processing.
* **Request Format**:
    * Multipart form-data containing the CSV file.
    * Headers:
        * `Content-Type: multipart/form-data`
    * Body: file: csvFile
* **Response Format**:
    * Success Response (HTTP 200):


```
{
  "request_id": "unique_request_id"
}

```



    * Error Response (HTTP 500):


```
{
  "error": "Validation error message"
}
```



#### **2. Status API**



* **Endpoint**: `/status/{request_id}`
* **Method**: `GET`
* **Description**: Allows clients to check the processing status of their requests using the request ID.
* **Request Format**:
    * Path Parameter:
        * `request_id`: The unique ID provided when the CSV was uploaded.
* **Response Format**:
    * Success Response (HTTP 200): 



```
{
  "request_id": "unique_request_id",
  "status": "current_status",
  "processed_rows": [
    {
      "unique_id": "unique_id",
      "original_urls": "original_image_urls",
      "processed_urls": "processed_image_urls"
    }
  ]
}

```



    * Error Response (HTTP 500):


```
{
  "error": "Error message indicating the issue"
}
```



#### **3. Webhook Handler**



* **Endpoint**: `/webhook`
* **Method**: `POST`
* **Description**: Listens for callbacks from the image processing service, updates the database with the processing results, and changes request statuses.
* **Request Format**:(Assumed)
    * Headers:
        * `Content-Type: application/json`
    * Body: 



```
{
  "request_id": "unique_request_id",
  "unique_id": "unique_row_id",
  "processed_urls": "processed_image_urls",
  "status": "completed"
}
```
