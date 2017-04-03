### DNS USAGE
Dillinger is very easy to install and deploy in a Docker container.

By default, the Docker will expose port 80, so change this within the Dockerfile if necessary. When ready, simply use the Dockerfile to build the image.

```sh
cd dillinger
docker build -t joemccann/dillinger:${package.json.version}
```
This will create the dillinger image and pull in the necessary dependencies. Be sure to swap out `${package.json.version}` with the actual version of Dillinger.

Once done, run the Docker image and map the port to whatever you wish on your host. In this example, we simply map port 8000 of the host to port 80 of the Docker (or whatever port was exposed in the Dockerfile):

```Java
        Name zone = Name.fromString("test.aegean.gr.");
        Name host = Name.fromString("mitsakos", zone);
        Update update = new Update(zone);
        update.delete(host);

        Resolver res = new SimpleResolver("83.212.113.216");

        res.setTSIGKey(null);
        //an theloume na ginei secure pprei na setaroume Secure Dynamic Update Process perissoters plirofories deite edw
        //https://technet.microsoft.com/en-us/library/cc961412.aspx
        res.setTCP(true);

        Message response = res.send(update);
        System.out.println(response.getHeader().toString());
```
DDDDDSS

```Java
Lookup lookup = null;
        try {
            lookup = new Lookup("83.212.113.216.test.aegean.gr", Type.TXT);
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
ssss
```sh
nslookup 83.212.113.216
```

``sh
Server:  dns-any-03.forthnet.gr
Address:  2a02:2148:82:8053::53

Name:    snf-665958.vm.okeanos.grnet.gr
Address:  83.212.113.216
```


