`curl --data "param1=value1&param2=value2" https://localhost:8080`

`curl --form "fileupload=@my-file.txt" https://localhost:8080`

`curl --form "fileupload=@my-file.txt;filename=desired-filename.txt" --form param1=value1 --form param2=value2 https://localhost:8080`

`curl --data '' https://localhost:8080`

`curl -X POST https://localhost:8080 -d "{"json": "test"}"`

`curl --request POST https://localhost:8080`
