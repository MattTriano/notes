# GCP

## Terms and Concepts

### Buckets
Buckets are the basic containers that hold data and everything in **Cloud Storage** must be in a bucket. Buckets must have a globally unique id and should have proper access control settings to avoid granting unintended access to the public.

Things stored in buckets are called **objects**.

### Objects
Each object has two components: **object data** and **object metadata**.

**Object data** is typically the file to be stored.

**Object metadata** is a collection of name-value pairs decribing features and qualitites of the object, including the object's name.

Objects are immutable, so the only operations for interacting with objects are creation, inspection, and deletion.


## How To

### Create a bucket

To create a bucket, go to the [GCP storage console](https://console.cloud.google.com/storage/) and click the "Create a bucket" button.

You'll have to:
* give your bucket a globally unique id,
* Select [where to store your data](https://cloud.google.com/storage/docs/locations), namely:
    * location [US, Europe, Asia], and 
    * location type [**Multi-Region** (multiple regions in the location), **Dual-Region** (two user-specified regions in the location), or (single) **Region**],
* Choose a [storage class](https://cloud.google.com/storage/docs/storage-classes):
    * **Standard** (Best for short-term storage and frequently accessed data)
    * **Nearline** (~1 access per month)
    * **Coldline** (~1 access per quarter)
    * **Archive** (~1 access per year)
* Decide if you want a hard barrier to [public access](https://cloud.google.com/storage/docs/public-access-prevention)
* Select an **access control** regime:
    * [Uniform bucket-level access](https://cloud.google.com/storage/docs/uniform-bucket-level-access)
    * [Fine-grained control](https://cloud.google.com/storage/docs/access-control/lists) through **Access Control Lists**
* Select a data protection option:
    * None,
    * [Object Versioning](https://cloud.google.com/storage/docs/object-versioning) (saves complete copies of different versions of objects),
    * [Retention Policy](https://cloud.google.com/storage/docs/bucket-lock) (useful for governance, objects can't be deleted before retention period expires)

### Connect to a bucket





























end
