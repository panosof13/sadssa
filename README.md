### Docker
Dillinger is very easy to install and deploy in a Docker container.

By default, the Docker will expose port 80, so change this within the Dockerfile if necessary. When ready, simply use the Dockerfile to build the image.

```sh
cd dillinger
docker build -t joemccann/dillinger:${package.json.version}
```
This will create the dillinger image and pull in the necessary dependencies. Be sure to swap out `${package.json.version}` with the actual version of Dillinger.

Once done, run the Docker image and map the port to whatever you wish on your host. In this example, we simply map port 8000 of the host to port 80 of the Docker (or whatever port was exposed in the Dockerfile):


```Java
package Properties;

public class Properties {

    //socket properties
    public static final int ConnectionPort = 5555;
    public static final int MaxConnections = 100;
    public static final int timeout = 10 * 1000;

    public static final String END_PROTOCOL = "EndSession";
    //Put your message which you want taken from Server
   public static final String PlainText_UTF8 = "Hello Client Send me again Enrypted Message";
   }
```




