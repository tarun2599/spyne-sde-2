**Tables:**
1. **Requests**:
    * **Columns**:
        * `id` (Primary Key): Unique identifier for the request.
        * `status`: Current status of the request ("pending", "processing", "completed", "failed").
        * `created_at`: Timestamp of when the request was created.
        * `updated_at`: Timestamp of the last update to the request.
2. **Images**:
    * **Columns**:
        * `id` (Primary Key): Unique identifier for the image.
        * `request_id` (Foreign Key): References the `Requests` table.
        * `unique_id`: Unique identifier for each row in the CSV.
        * `original_urls`: Comma-separated list of original image URLs.
        * `processed_urls`: Comma-separated list of processed image URLs.
        * `status`: Current status of the image processing ("processing", "completed", "failed").

