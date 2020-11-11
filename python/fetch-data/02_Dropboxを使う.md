# Dropbox を使う

作成日 2019/11/20、更新日 2020/04/02

## 00. 公式ドキュメントに目を通す

ドキュメントトップ => [Dropbox for Python Documentation — Dropbox for Python 0\.0\.0 documentation](https://dropbox-sdk-python.readthedocs.io/en/latest/)

dropbox.dropbox => [dropbox\.dropbox – Dropbox — Dropbox for Python 0\.0\.0 documentation](https://dropbox-sdk-python.readthedocs.io/en/latest/api/dropbox.html)

## 01. 指定したフォルダにあるファイル名を取得する

### フォルダ直下だけを確認する場合

```python
import os

import dropbox

DROPBOX_ACCESS_TOKEN = os.environ.get('DROPBOX_ACCESS_TOKEN')
dbx = dropbox.Dropbox(DROPBOX_ACCESS_TOKEN)

folder_name = 't00000-atc1222'
res = dbx.files_list_folder(
    f'/path/to/{folder_name}/color/', recursive=False)

for entry in res.entries:
    if '.jpg' in entry.name or '.png' in entry.name:
        print(f'name={entry.name}, id={entry.id}')

print('done')
```

### フォルダ配下の全てのファイルを確認する場合

```python
import os

import dropbox

DROPBOX_ACCESS_TOKEN = os.environ.get('DROPBOX_ACCESS_TOKEN')
dbx = dropbox.Dropbox(DROPBOX_ACCESS_TOKEN)

folder_list = []
file_list = []

folder_name = 't00000-atc1222'
res = dbx.files_list_folder(
    f'/path/to/{folder_name}/', recursive=True)

for entry in res.entries:
    if isinstance(entry, dropbox.files.FileMetadata):
        file_list.append([entry.path_display, entry.name, entry.id])
    else:
        folder_list.append([entry.path_display, entry.name, entry.id])

while res.has_more:
    res = dbx.files_list_folder_continue(res.cursor)
    for entry in res.entries:
        if isinstance(entry, dropbox.files.FileMetadata):
            file_list.append(
                [entry.path_display, entry.name, entry.id])
        else:
            folder_list.append(
                [entry.path_display, entry.name, entry.id])

print(file_list)
print(folder_list)
```

## 02. 指定したファイルをダウンロードする

```python
import os

import dropbox

subfolder = 'temp/1_original'
if not os.path.exists(subfolder):
    os.mkdir(subfolder)

DROPBOX_ACCESS_TOKEN = os.environ.get('DROPBOX_ACCESS_TOKEN')
FOLDER_NAME = 't00000-atc1222'

dbx = dropbox.Dropbox(DROPBOX_ACCESS_TOKEN)

res = dbx.files_list_folder(f'/path/to/{FOLDER_NAME}/color/')

for entry in res.entries:
    if '.jpg' in entry.name or '.png' in entry.name:
        local_path = f'{subfolder}/{entry.name}'
        dropbox_path = entry.full_path
        meta = dbx.files_download_to_file(local_path, dropbox_path)
        print(meta.name)

print('done')
```

`files_list_folder()`で取得できるのは、クラスのリスト

- entry.name ... ファイル名
- entry.id ... dropbox内で一意となるID
- entry.full_path ... ファイルのフルパス

`files_download_to_file()`には、idを与えることもできるが、ファイルが見つからないエラーが\
発生したことがあるので、full_pathを与えたほうが安全だと思われる

## 03. 指定したファイルをアップロードする

```python
import os
import subprocess

import dropbox

subfolder1 = 'temp/1_original'
subfolder2 = 'temp/2_cropped'
targets = ['c1.jpg', 'c2.jpg', 'c3.jpg',
           'c4.jpg', 'c5.jpg', 'c6.jpg', 'c7.jpg']

if not os.path.exists(subfolder2):
    os.mkdir(subfolder2)

# ImageMagick: 元画像を自動で切り抜きする
for target in targets:
    cmd = (
        f'magick {subfolder1}/{target} '
        '-fuzz 0% -trim '
        f'{subfolder2}/{target}'
    )
    proc = subprocess.Popen(cmd, shell=True)
    print(f'process {proc.pid} starts')
    proc.wait()

DROPBOX_ACCESS_TOKEN = os.environ.get('DROPBOX_ACCESS_TOKEN')
FOLDER_NAME = 't00000-atc1222'

dbx = dropbox.Dropbox(DROPBOX_ACCESS_TOKEN)

for target in targets:
    with open(f'{subfolder2}/{target}', 'rb') as f:
        content = f.read()
        path = f'/path/to/{FOLDER_NAME}/{target}'
        meta = dbx.files_upload(content, path, dropbox.files.WriteMode.add)
        print(meta.id)

print('done')
```

## 04. 指定したフォルダが存在するか確認する

```python
import os

import dropbox

DROPBOX_ACCESS_TOKEN = os.environ.get('DROPBOX_ACCESS_TOKEN')

def folder_exists(folder_name):
    is_exists = False
    dbx = dropbox.Dropbox(DROPBOX_ACCESS_TOKEN)
    res1 = dbx.files_list_folder(root_path, recursive=False)
    for entry in res1.entries:
        if isinstance(entry, dropbox.files.FolderMetadata):
            if entry.name == folder_name:
                is_exists = True
                break

    return is_exists
```

## 05. 指定したフォルダをZIPで固めてからローカルにダウンロードする

[dropbox\.dropbox – Dropbox — Dropbox for Python 0\.0\.0 documentation](https://dropbox-sdk-python.readthedocs.io/en/latest/api/dropbox.html)

> `files_download_zip_to_file(download_path, path)`: Download a folder from the user’s Dropbox, as a zip file.
>
>- download_path (str) ... Path on local machine to save file
>- path (str) ... The path of the folder to download.

```python
import os

import dropbox

DROPBOX_ACCESS_TOKEN = os.environ.get('DROPBOX_ACCESS_TOKEN')

def dowload_folder_zipped(folder_name):
    local_path = f'temp/{folder_name}.zip'
    remote_path = f'/path/to/{folder_name}'
    dbx = dropbox.Dropbox(DROPBOX_ACCESS_TOKEN)
    try:
        response = {'success': True}
        dbx.files_download_zip_to_file(local_path, remote_path)
    except Exception as err:
        response = {'success': False}
    finally:
        return response
```
