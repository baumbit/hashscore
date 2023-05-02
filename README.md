# HashScore

A HashScore is a simple signed data structure, which ties a hash to one or several scores. A suitable [hash](https://en.wikipedia.org/wiki/Cryptographic_hash_function) could be SHA-512, and a score is always a floating point value 0-1. The purpose of HashScore is to leverage users [Web-of-Trust](https://en.wikipedia.org/wiki/Web_of_trust) to sort and filter digital resources.

## The problem
While internet enables decentralised applications, it does not and can not know the validity (in lack of better terms) of the digital resources on it. Protocols such as TCP/IP, UDP, etc., can not discern what is true, what is false and/or what is important. Internet users often find themselves depending on selected authorities to weed this out, which introduces several weaknesses. Perhaps most recognised is that its often implemented as centralised entities which can be DDoS attacked and/or hacked to spread disinformation. Another less recognised weakness, is that attackers can flood internet with data, to crowd out and thereby divert the attention of users from data that which is important. Furthermore, what is important depends on the context and is highly subjective, thus there is no objective provable truth.

## The solution
A HashScore can be formatted as a JSON-object and can be stored on any machine connected to internet. These machines can expose an API that makes it possible for other machines to upload and download the HashScore. Together these machines creates a network of nodes. It matters less if this API is implemented as a stand alone service or if it is part of some other app API. The important thing is that nodes on the network can access the API and download the the HashScore from several different nodes. By collecting many HashScore of a specific digital resource, the collection of HashScores can be parsed and a final score be calculated. The final score indicates how the group in its totality scores the digital resource.

## Can a HashSore be trusted?
As long as the hash-function or the cryptographic signature is not broken, a HashScore can not be hacked. The cryptographic math, ensures that even if the node that stores the HashScore is hacked, the HashScore itself can not be hacked. However, the creator of a HashScore can both lie, be hacked or simply just be wrong, hence as a user you should download several HashScores created by several different creators that can be trusted. If one of those is often wrong, lies or for some other reason is unreliable, the creator should not longer be trusted and probably be replaced. So yes, a HashScore can be trusted as long as you as a user are downloading several HashScores, preferably from several different (to ensure that you get the latest updated version of the HashScore) sources.

Note: The HashScore is only as good as the peers you have put your trust in. Also you must shake hands (exchange cryptographic keys) before you can verify the signature. Should your key leak, it can no longer be trusted. By creatig a master key, you can then create key derivatives which can be hashed and now your peers can verify that you (do not suspect) that your key have been leaked (yet). It's not 100% secure, because the master key can also be leaked, but its more secure and they master key can be split up and stored in a way that is more secure.  

## Can a HashScore be mutated?
A HashScore can not be changed after it has been created, without re-signing the HashScore. However a HashScore may contain one or several different types of scores. In a JSON-object the type of score, is inferred by the name of the property. Example of a HashScore with two different types of scores:
```
{
hashscore: {
sha512: <hash of the digital resource>,
uri: <resource identifier>,
created: <signing timestamp>,
trust: <0-1>, // example: a score indicating if the hashed resource can be trusted
priority: <0-1> // example: a score indicating how importance of the hashed resource
}
signature: <the signed hashscore>
}
```

## Use-case examples

### Torrents
Does the torrent file contain the data the the name of the file suggests? A HashScore with a trust of 0 indicates it does not. A score of 1 indicates that it does. By querying what your trusted peers (other people you trust) thinks, you can now make a qualified guess.

### App downloaded from internet
Can I trust that the app I downloaded from internet does not contain a virus? Consult your trusted peers to find out if the URI matches the same hash you have and if it can be trusted or not.

### Social Media
Is the news from the news outlet, or message on social media, important for me to read or not? Query your trusted peers for their opinion on the priority and then create a filtered list of content. For a deep dive about this, read more about [PeerCuration](https://github.com/baumbit/peercuration).

### Nostr
Nostr could benefit from HashScore in many ways. It could be used to mitigate issues related to accounts being hacked (could fascilitate account migration) and protect against DDoS attack by leveraging [PeerCuration](https://github.com/baumbit/peercuration).
