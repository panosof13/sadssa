### DNS USAGE
Domain Name System (DNS) client computers can use dynamic update to register and dynamically update their resource records with a DNS server whenever changes occur. This reduces the need for manual administration of zone records, especially for clients that frequently move or change locations and use Dynamic Host Configuration Protocol (DHCP) to obtain an IP address.

Dynamic updates are sent or refreshed periodically. By default, computers send a refresh once every seven days. If the update results in no changes to zone data, the zone remains at its current version and no changes are written. Updates result in actual zone changes or increased zone transfer only if names or addresses actually change.

When the DNS Client service registers host (A) and pointer (PTR) resource records for a computer, it uses a default caching Time to Live (TTL) of 15 minutes for host records. This determines how long other DNS servers and clients cache a computer's records when the records are included in a query response.


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
---
This lookup will list DNS Text (TXT) records for a domain. The DNS lookup is done directly against the domain's authoritative name servers, so changes to DNS TXT Records should show up instantly.As we can see below if we run the given code with the correct DNS Zone record that we create before as well as with the same resolver,we will get instantly DNS TXT Records.

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
---
In order to test the lifecycle of our process we can open a cmd terminal and the then we can type the below command.

``
$ nslookup 83.212.113.216
```

---

``
$ Server:  dns-any-03.forthnet.gr
$ Address:  2a02:2148:82:8053::53

$ Name:    snf-665958.vm.okeanos.grnet.gr
$ Address:  83.212.113.216
```
---


