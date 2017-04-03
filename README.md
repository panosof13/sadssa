### DNS USAGE
  <p>Domain Name System (DNS) client computers can use dynamic update to register and dynamically update their resource records with a DNS server whenever changes occur. This reduces the need for manual administration of zone records, especially for clients that frequently move or change locations and use Dynamic Host Configuration Protocol (DHCP) to obtain an IP address.</p>
 <p>Dynamic updates are sent or refreshed periodically. By default, computers send a refresh once every seven days. If the update results in no changes to zone data, the zone remains at its current version and no changes are written. Updates result in actual zone changes or increased zone transfer only if names or addresses actually change.</p>
 <p> When the DNS Client service registers host (A) and pointer (PTR) resource records for a computer, it uses a default caching Time to Live (TTL) of 15 minutes for host records. This determines how long other DNS servers and clients cache a computer's records when the records are included in a query response.</p>


## Example
The following example demonstrates how easy will be to update DNS Records with TXT usage. A TXT Record is a resource record in the Domain Name System (DNS) used to hold some plaintext information about a particular hostname/zone provided to sources outside your domain.

```Java
        Name zone = Name.fromString("test.aegean.gr.");
        Name host = Name.fromString("mitsakos", zone);
        Update update = new Update(zone);
        update.resolve(host, Type.TXT, 600, "asdass.test.aegean.gr");

        Resolver res = new SimpleResolver("83.212.113.216");

        res.setTSIGKey(null);
        //an theloume na ginei secure pprei na setaroume Secure Dynamic Update Process perissoters plirofories deite edw
        //https://technet.microsoft.com/en-us/library/cc961412.aspx
        res.setTCP(true);

        Message response = res.send(update);
        System.out.println(response.getHeader().toString());
```
---
<p>This lookup will list DNS Text (TXT) records for a domain. The DNS lookup is done directly against the domain's authoritative name servers, so changes to DNS TXT Records should show up instantly. As we can see below if we run the given code with the correct DNS Zone record that we create before as well as with the same resolver,we will get instantly DNS TXT Records.</p>

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
<p>In order to test the lifecycle of our process we can open a cmd terminal and the then we can type the below command.</p>
   * At a command prompt, type Nslookup, and then press ENTER.
   * Type server  address, where IP address is the IP address of your external DNS server.

```sh
$ nslookup 83.212.113.216
```
<p> The following example shows how the DNS server  resolves the IP address of the external domain without affect with informations the DNS TXT records.</p>

```sh
$ Server:  dns-any-03.forthnet.gr
$ Address:  2a02:2148:82:8053::53

$ Name:    snf-665958.vm.okeanos.grnet.gr
$ Address:  83.212.113.216
```
---


