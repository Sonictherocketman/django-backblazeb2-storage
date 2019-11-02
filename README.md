BACKBLAZE B2 Storage for Django
================================

BackBlaze B2 Storage for django.

### Installation

via PIP:

    pip install django-backblazeb2-storage

Manual:

    pip install git+git://github.com/royendgel/django-backblazeb2-storage.git

### Usage

Add these to your Django app's settings

    BACKBLAZEB2_APP_KEY_ID = 'your-app-key-id'
    BACKBLAZEB2_APP_KEY = 'your-app-key'
    BACKBLAZEB2_BUCKET_NAME = 'bucketname'
    BACKBLAZEB2_BUCKET_ID = 'bucketid'

To make it your default django storage:

    DEFAULT_FILE_STORAGE = 'b2_storage.storage.B2Storage'

Due to the way B2 requires information from the Storage to retrieve and delete files, the FileField value contains a hybrid of 2 values. These are hidden by the Storage API, but they do require that the FileField have a longer than default `max_length` value. The recommended setting is below:

    class UserUpload(models.Model):
        file = models.FileField(upload_to='media/', max_length=500)

### Managing Authorization Requests

Backblaze authorization tokens expire after "at most 24 hours" according to the Backblaze documentation. This means that we need to periodically reauthorize ourselves against Backblaze. By default, a new authorization token is retrieved after 1 hour. Any requests made by the Storage class in that hour will use the same token. If this setting is not ideal for your uses, you can override the setting in your `settings.py` like so:

`BACKBLAZEB2_AUTHORIZATION_BUFFER = timedelta(hours=23)`

**Note: This setting should always be less than 24 hours according to the Backblaze documentation.**

For more information, [see b2_authorize_account](https://www.backblaze.com/b2/docs/b2_authorize_account.html)
