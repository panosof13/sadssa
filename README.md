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
Lookup lookup = null;
        try {
            lookup = new Lookup("test.aegean.gr", Type.TXT);
            lookup.setResolver(new SimpleResolver("83.212.113.216"));
            Record[] records = lookup.run();
            if (lookup.getResult() != Lookup.SUCCESSFUL && lookup.getResult() != Lookup.TYPE_NOT_FOUND) {
                System.out.println("Error");
            } else {
                for (Record rec : records) {
                    {
                        if (rec instanceof TXTRecord) {
                            TXTRecord txt = (TXTRecord) rec;
                            for (Iterator j = txt.getStrings().iterator(); j.hasNext();) {
                                responseMessage = (String) j.next();
                            }
                        } else if (rec instanceof ARecord) {
                            listingType = ((ARecord) rec).getAddress().getHostAddress();
                        }
                    }
                    System.out.println("Found!");
                    System.out.println("Response Message: " + responseMessage);
                    System.out.println("Listing Type: " + listingType);
                }
            }

        } catch (TextParseException e) {
            throw new RuntimeException(e);
        }
```




