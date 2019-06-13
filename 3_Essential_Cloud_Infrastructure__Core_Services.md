# Core Services

## Cloud IAM

- Who - can do what - on which resource
- Cloud IAM Objects: 
  - Organization:
    - Organization Owners:
      - Established at creation of the Organization by Google Sales
      - GSuite Super Admins are the only Organization Owners
      - assign the Organization Administrator role from the GSuite Admin Console (Admin is a separate product)
    - Roles:
      - Organization Admin
      - Viewer
      - Project Creator
    - Beqt practice: Use Groups

  - Folders:
    - Roles:
      - Admin
      - Creator
      - Viewer
  
  - Projects:
    - Creator
    - Deleter
  
  - Members:
    - user accounts, groups
    - Service accounts:
      - User created (custom): support Cloud IAM roles only
      - Built-in (Compute Engine and App Engine defaults service accounts): suppoert Cloud IAM roles and primitive roles
      - Google APIs service accounts (runs internal Google processes on your behalf)
    - Use scope for built-in service accounts and IAM roles for user-created ones
    - Roles for Service accounts can be assigned to groups or users
    - Service accounts authenticate using keys
      - Google managed keys
      - User managed keys

  - Roles
    - Primite roles (legacy):
      - Owner
      - Editor
      - Viewer
      - & Billing Adminstrator
    - Predefined roles (granular access) - are list of permissions:
      - ex: InstanceAdmin role -> List of permissions:
        - compute.instances.delete
        - compute.instances.get
        - compute.instances.list
        - compute.instances.setMachineType
        - compute.instances.start
        - compute.instances.stop
        - ...
    - Custom roles:
      - custom

    - Product specific roles:
      - ![Product roles](img/product-roles.png)
  - Resources
  - Products
  - G Suite Super Admins
  - IAM best practices:
    - Use resource hierarchy
    - Granting roles to groups instead of individuals. This allows to update group membership instead of changing a Cloud IAM policy

### Cloud Storage
- Storage class can be changed form Regional to Nearline and Coldline and vice versa
- Storage class can be changed form Multi-Regional to Nearline and Coldline and vice versa
- Storage class can not be changed from Regional to Multi-Regional and vice versa
- Access Control Lists and Signed urls
  - ACLs:
    - Scopes:
      - allUsers
      - allAuthenticatedUsers
      - Google accounts (individuals, group, domains)
      - Cloud storage ID
      - Project conveniance values :
        - owners-[CS project ID]
        - editors-[CS project ID]
        - viewers-[CS project ID]
    - Permissions:
      - Owner
      - Writer
      - Viewer
  - Signed Urls:
    - Time limited
    - `gsutil signurl -d 10m path/to/privatekey.p12 gs://bucket/object`

- Cloud storage features:
  - Customer-supplied encryption keys
  - Object lifecycle management. examples:
    - Downgrade storage class on objects older than one year
    - Delete objects created before a specific date
    - Keep only the 3 most recent versions of an object
    Changes to lyfecycle configurations can take 24h to apply 
  - Object versionning
  - Directory synchronization (synchronizes a VM directory with a bucket)
  - Object change notification (Pub/Sub)
  - Data import
    - Storage Transfert Service
    - Google Transfer Appliance
    - Offline Media Import
  - Strong consistency

### Lab Cloud Storage

#### Copy file to bucket

```bash
gsutil cp setup.html gs://$BUCKET_NAME_1/
```

#### Get default access list

```bash
gsutil acl get gs://$BUCKET_NAME_1/setup.html  > acl.txt
cat acl.txt
```

#### cat acl.txt

```json
[
  {
    "entity": "project-owners-867726970068",
    "projectTeam": {
      "projectNumber": "867726970068",
      "team": "owners"
    },
    "role": "OWNER"
  },
  {
    "entity": "project-editors-867726970068",
    "projectTeam": {
      "projectNumber": "867726970068",
      "team": "editors"
    },
    "role": "OWNER"
  },
  {
    "entity": "project-viewers-867726970068",
    "projectTeam": {
      "projectNumber": "867726970068",
      "team": "viewers"
    },
    "role": "READER"
  },
  {
    "email": "google3400584_student@qwiklabs.net",
    "entity": "user-google3400584_student@qwiklabs.net",
    "role": "OWNER"
  }
]
```



#### Set the access list to private and verify the results

```bash
gsutil acl set private gs://$BUCKET_NAME_1/setup.html
gsutil acl get gs://$BUCKET_NAME_1/setup.html  > acl2.txt
cat acl2.txt
```


#### cat acl2.txt

```json
[
  {
    "email": "google3400584_student@qwiklabs.net",
    "entity": "user-google3400584_student@qwiklabs.net",
    "role": "OWNER"
  }
]
```

#### To update the access list to make the file publicly readable

```bash
gsutil acl ch -u AllUsers:R gs://$BUCKET_NAME_1/setup.html
gsutil acl get gs://$BUCKET_NAME_1/setup.html  > acl3.txt
cat acl3.txt
```

#### cat acl3.Txt

```json
[
  {
    "email": "google3400584_student@qwiklabs.net",
    "entity": "user-google3400584_student@qwiklabs.net",
    "role": "OWNER"
  },
  {
    "entity": "allUsers",
    "role": "READER"
  }
]
```

Customer-supplied Encryption Keys (CSEK)

#### For the next step, you need an AES-256 base-64 key.

```bash
python -c 'import base64; import os; print(base64.encodestring(os.urandom(32)))'
```

```bash
If the .boto file is empty, close the nano editor with Ctrl+X and generate a new .boto file using the gsutil config -n command. Then, try opening the file again with the above commands.

If the .boto file is still empty, you might have to locate it using the gsutil version -l command.
```

#### Upload the remaining setup files (encrypted) and verify in the GCP Console

```bash
gsutil cp gs://$BUCKET_NAME_1/setup* ./
```

#### To cat the encrypted files to see whether they made it back, run the following commands:

```bash
cat setup.html
cat setup2.html
cat setup3.html
```

Rotate CSEK keys

#### Change encryption_key and add decryption_key1 with the old encryption_key (nano .boto)

#### Rewrite the key for file 1 and comment out the old decrypt key
```md
When a file is encrypted, rewriting the file decrypts it using the decryption_key1 that you previously set, and encrypts the file with the new encryption_key.

You are rewriting the key for setup2.html, but not for setup3.html, so that you can see what happens if you don't rotate the keys properly
```

```bash
gsutil rewrite -k gs://$BUCKET_NAME_1/setup2.html
```

#### Comment descryption_key1
#### Download setup2 and setup3

```bash
gsutil cp  gs://$BUCKET_NAME_1/setup2.html recover2.html
```

```bash
gsutil cp  gs://$BUCKET_NAME_1/setup3.html recover3.html
```

```md
What happened? setup3.html was not rewritten with the new key, so it can no longer be decrypted, and the copy will fail.

You have successfully rotated the CSEK keys.
```

Enable lifecycle management

#### View the current lifecycle policy

```bash
gsutil lifecycle get gs://$BUCKET_NAME_1
```

#### Create a life.json

```bash
nano life.json
```

```json
{
  "rule":
  [
    {
      "action": {"type": "Delete"},
      "condition": {"age": 31}
    }
  ]
}
```

#### Set the policy and verify

```bash
gsutil lifecycle set life.json gs://$BUCKET_NAME_1
```

```bash
gsutil lifecycle get gs://$BUCKET_NAME_1
```

Enable versioning

#### View the versioning status for the bucket and enable versioning

```bash
gsutil versioning get gs://$BUCKET_NAME_1
```

```bash
gsutil versioning set on gs://$BUCKET_NAME_1
```

```bash
gsutil versioning get gs://$BUCKET_NAME_1
```

#### Create several versions of the sample file in the bucket

```bash
ls -al setup.html
```

```bash
nano setup.html

Delete 5 lines to reduce file size
```

#### Copy the file to the bucket with the -v versioning option

```bash
gsutil cp -v setup.html gs://$BUCKET_NAME_1
```

#### List all versions of the file

```bash
gsutil ls -a gs://$BUCKET_NAME_1/setup.html
```

```bash
export VERSION_NAME=<Enter VERSION name here>
```

```bash
gsutil cp $VERSION_NAME recovered.txt
```

Synchronize a directory to a bucket

#### Create nested directory

```bash
mkdir firstlevel
mkdir ./firstlevel/secondlevel
cp setup.html firstlevel
cp setup.html firstlevel/secondlevel
```

#### Sync the firstlevel directory on the VM with the bucket

```bash
gsutil rsync -r ./firstlevel gs://$BUCKET_NAME_1/firstlevel
```

```bash
gsutil ls -r gs://$BUCKET_NAME_1/firstlevel
```

```bash
exit
```

Cross-project sharing

#### Activate service account from VM ssh

```bash
gcloud auth activate-service-account --key-file credentials.json
```

### Cloud SQL


####

```bash

```

```bash

```

####

```bash

```

```bash

```

####

```bash

```

```bash

```

####

```bash

```

```bash

```

####

```bash

```

```bash

```

####

```bash

```

```bash

```

####

```bash

```

```bash

```