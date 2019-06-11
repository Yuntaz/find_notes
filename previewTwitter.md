# Workaround to preview Twitter on Find

1. Stop the Find service
```
sudo systemctl stop find
```
2. Go to the Find home
```
cd /opt/Find/home
```
3. Backup up it
```
cp config.json config.json_backup_twitter
```
4. Edit the config.json file
```
vi config.json
```
5. Go to the section in the Json file called <b>previewWhitelistUrls</b>
```
ESC ESC /previewWhitelistUrls
```
6. Add this line inside the collection of strings of <b>previewWhitelistUrls</b>. Please, if it is last, don't add a last comma, otherwise, we need to add a final comma after our line.
```
"^(https?://)?twitter\\.com/.*" : "<iframe class=\"preview-document-frame\" allow=\"autoplay; encrypted-media\" allowfullscreen src=\"https://twitframe.com/show?url=<%-reference%>\">"
```
7. Save file
```
ESC ESC :wq!
```
8. Start the service
```
sudo systemctl start find
```

