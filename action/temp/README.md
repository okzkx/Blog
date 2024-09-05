临时文件存放处

Pixso , 记录思维导图


``
```
#!/bin/bash

  

git fetch origin

git checkout develop

git reset --hard origin/develop

  

git submodule foreach --recursive 'git fetch origin'

git submodule foreach --recursive 'git checkout develop'

git submodule foreach --recursive 'git reset --hard origin/develop'

  

read -p "Press Enter to continue..."
```

