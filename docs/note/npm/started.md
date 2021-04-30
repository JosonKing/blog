# npm 基础命令

> install & version

```
npm -v

npm install npm@latest -g

npm install npm@next -g

npm install <package_name>
```

> init

```
npm init

npm init -y

npm set init.author.email "wombat@npmjs.com"
npm set init.author.name "ag_dubs"
npm set init.license "MIT"
```

> package.json

```
{
  "name": "name_name-name",
  "version": "1.0.0"
}
```

> install

```
npm install [-g] <package>
```

```
#dependencies
npm install <package_name> --save

#devDependencies
npm install <package_name> --save-dev
```

> update

```
npm update [-g] [<pkg>...]

npm update

npm update -g <package>

npm update --save [<pkg>...]
```

> uninstall

```
npm uninstall [-g] [<package>]

npm uninstall --save <package>

npm uninstall -save-dev <package>
```
