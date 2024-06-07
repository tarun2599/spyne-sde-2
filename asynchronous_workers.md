### **Asynchronous Workers: Description**


#### **Worker Nodes:**

1. **Roles**:
    * Retrieve tasks from the asynchronous task queue.
    * Process CSV data and submit to the image processing service.
    * Update the database in images collection when images in the selected row go to processing.
2. **Functions**:
    * Fetch tasks from the queue.
    * Pass CSV rows and send image URLs to the image processing service.
    * Update processing status in the database.
3. **Description**:
    * Worker nodes are responsible for the actual processing of tasks in the system.
    * They continuously listen to the asynchronous task queue for incoming tasks.
    * Once a task is received, they fetch the necessary data (CSV rows) and start processing.
    * Tasks are processed asynchronously to ensure scalability and efficiency.
    * Worker nodes pass CSV rows which contains image urls to the image processing service.
    * They handle the responses from the image processing service and update the database with the processing results.
    * Worker nodes also update the status of tasks in the database, marking them as "processing", "completed", or "failed" based on the outcome of the processing.
    * These nodes are scalable and can be deployed across multiple instances to handle high loads and concurrent processing tasks.
4. **Example Scenario**:
    * When a CSV file is uploaded, the worker nodes receive tasks from the queue.
    * Each worker node fetches a task and processes a portion of the CSV data.
    * They call the image processing service to transform the images asynchronously.
    * If any errors occur during processing, they handle them gracefully and update the task status accordingly.
5. **Implementation**:
    * Can be implemented using various technologies such as Apache Kafka
    * Worker nodes are typically long-running processes or services that continuously listen for incoming tasks.
    * They can be deployed on separate servers or containers to distribute the processing load and ensure fault tolerance.

