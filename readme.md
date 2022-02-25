## Apache+Tomcat6 Web Container Sample

# 起動
```
docker-compose up
```

# デーモン起動
docker-compose up -d

# imageを構築
docker-compose up --build

# 関連の破棄
```
docker-compose down --rmi all --volumes
```

リポジトリに追加
```
git init
git add *
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/refrain62/docker_apache_tomcat6_sample.git
git push -u origin main
```
