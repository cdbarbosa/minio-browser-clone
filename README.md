# MinIO - Changing the browser style

MinIO has three ways to run.

1. Using the Docker Container;

2. Downloading the MinIO package;

3. Cloning the repository and, from the supplied Dockerfile, make a ```docker build .``` and then a ```docker run image_name```

The third option provides the API codes used by MinIO, in Go lang, and the browser codes, in React JS.

In this case, the best way to make styling changes is to clone the repository and make changes directly to the code.

```sh
git clone https://github.com/minio/minio.git
cd minio
```

### Install node

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
exec -l $SHELL
nvm install stable
```

### Install node dependencies

```sh
cd browser
npm install
```

### Run MinIO Browser

```sh
npm run dev
```

MinIO Browser is already running and code modifications can be made, which will be shown on the screen. However, running only the browser code, the api is not running, so the main features will not work.

### Run MinIO Server

To run MinIO Server it is necessary to download the MinIO package and run it locally.

## macOS

### Homebrew (recommended)

Install minio packages using [Homebrew](https://brew.sh/)

```sh
brew install minio/stable/minio
minio server /data
```

> NOTE: If you previously installed minio using `brew install minio` then it is recommended that you reinstall minio from `minio/stable/minio` official repo instead.
>
```sh
brew uninstall minio
brew install minio/stable/minio
```

### Binary Download

| Platform    | Architecture | URL                                                       |
| ----------  | --------     | ------                                                    |
| Apple macOS | 64-bit Intel | <https://dl.min.io/server/minio/release/darwin-amd64/minio> |

```sh
chmod 755 minio
./minio server /data
```

## GNU/Linux

### Binary Download

| Platform   | Architecture | URL                                                      |
| ---------- | --------     | ------                                                   |
| GNU/Linux  | 64-bit Intel | <https://dl.min.io/server/minio/release/linux-amd64/minio> |

```sh
wget https://dl.min.io/server/minio/release/linux-amd64/minio
chmod +x minio
./minio server /data
```

| Platform   | Architecture | URL                                                        |
| ---------- | --------     | ------                                                     |
| GNU/Linux  | ppc64le      | <https://dl.min.io/server/minio/release/linux-ppc64le/minio> |

```sh
wget https://dl.min.io/server/minio/release/linux-ppc64le/minio
chmod +x minio
./minio server /data
```

## Microsoft Windows

### Binary Download

| Platform          | Architecture | URL                                                            |
| ----------        | --------     | ------                                                         |
| Microsoft Windows | 64-bit       | <https://dl.min.io/server/minio/release/windows-amd64/minio.exe> |

```sh
minio.exe server D:\Photos
```

After running MinIO Server, it is necessary to make a modification to the `web.js` file that is located in the Browser folders (`browser/app/js/web.js`)

The modification must be carried out on line 136.

What appears in this line is the endpoint, where the browser will look for the API. At this point, it points to a url that is made up of the same url as the Browser. However, in this type of execution the browser and the server are running separately. So, it is necessary to make a replacement, so that the browser sees the server at the endpoint that is being executed.

When the git clone of the MinIO project is made, the file has the following line:

```js
const web = new Web(`${window.location.protocol}//${window.location.host}${minioBrowserPrefix}/webrpc`);
```

The `${window.location.host}` is the endpoint of the Browser itself, and what should be replaced.

When running the MinIO server, the server's endpoint will appear in the terminal as follows:

```sh
Endpoint:  http://192.168.0.104:9000  http://172.17.0.1:9000  http://127.0.0.1:9000        
AccessKey: minioadmin 
SecretKey: minioadmin 
```

> Note: Accesskey and SecretKey are initially standard. You can update it later.

So, `192.168.0.104: 9000` this is the endpoint that will have to be replaced in place of the `${window.location.host}`

After this replacement, the browser functionality will be working normally and the styling changes will be reflected on the screen.
