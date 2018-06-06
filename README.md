# commands
This are list of useful command which i need to google again and again.

# unziping 
Open a terminal (Ctrl + Alt + T should work).
Now create a temporary folder to extract the file: mkdir temp_for_zip_extract.
Let's now extract the zip file into that folder:
unzip /path/to/file.zip -d temp_for_zip_extract

# virtual environment
1) https://medium.com/init27-labs/installation-of-opencv-using-anaconda-mac-faded05a4ef6
at least creater one environment from it.

# installing Opencv in Mac OS is pain: 
1) https://www.pyimagesearch.com/2016/12/05/macos-install-opencv-3-and-python-3-5/
will help, for 3.6 and above see comments.



# On using google collab

# downloading, uploading file from Google Drive
This i found in stackoverflow...

!pip install -U -q PyDrive
import os
from pydrive.auth import GoogleAuth
from pydrive.drive import GoogleDrive
from google.colab import auth
from oauth2client.client import GoogleCredentials

# 1. Authenticate and create the PyDrive client.
auth.authenticate_user()
gauth = GoogleAuth()
gauth.credentials = GoogleCredentials.get_application_default()
drive = GoogleDrive(gauth)

# this will ask for authentication, infact you can use any gdrive account,
# maybe you are running collab from account x but your file store in account y
# just authenticate account y, this should work fine

# choose a local (colab) directory to store the data.
local_download_path = os.path.expanduser('~/data')
try:
  os.makedirs(local_download_path)
except: pass

# 2. Auto-iterate using the query syntax
#    https://developers.google.com/drive/v2/web/search-parameters
file_list = drive.ListFile(
    {'q': "'1jHEww8CxVw5p4D-vU1IankQfn5KmPzFi' in parents"}).GetList()
# 1jHEww8CxVw5p4D-vU1IankQfn5KmPzFi is id assigned to a folder. which you can find in address bar by click at any file in the G drive folder

for f in file_list:
  # 3. Create & download by id.
  print('title: %s, id: %s' % (f['title'], f['id']))
  fname = os.path.join(local_download_path, f['title'])
  print('downloading to {}'.format(fname))
  f_ = drive.CreateFile({'id': f['id']})
  f_.GetContentFile(fname)


##################################
# little extra upload back the file to g drive
uploaded = drive.CreateFile({'title': 'filename_xyz.some'})
uploaded.SetContentFile('filename_xyz.some')
uploaded.Upload()
print('Uploaded file with ID {}'.format(uploaded.get('id')))
