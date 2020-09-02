# Obtain PubKey from signature (C++)

## service environment：
     WaykiChain:3.2.1  
     download url: https://github.com/WaykiChain/WaykiChain.git

## test way
     using rpc

## test step
    1. Get pubKey named Key1 from privateKey
    2. Get pubKey named Key2 from hash and sign
    3. To compare Key1 and Key2

## input code
```
bool test()
{
    cout << "hello test ok !" << endl;

    CCoinSecret priSecret;
    priSecret.SetString("Y6J4aK6Wcs4A3Ex4HXdfjJ6ZsHpNZfjaS4B9w7xqEnmFEYMqQd13");
    
    // get publicKey from private
    CKey key = priSecret.GetKey();
    CPubKey pubkey = key.GetPubKey();

    cout << "pubKey = " << pubkey.GetKeyId().ToString()<< endl;
    
    // hash
    string strMsg = "hello world";
    uint256 hashMsg = Hash(strMsg.begin(), strMsg.end());

    // sign
    vector<unsigned char> csign;
    key.SignCompact(hashMsg, csign);

    // get public key from hash and sign
    CPubKey rKey;
    rKey.RecoverCompact(hashMsg, csign);
    cout << "rKey = " << rKey.GetKeyId().ToString() << endl;

    bool aresult = (rKey == pubkey ? true :false);
    cout << "aresult = " << aresult << endl;


    return true;
}

```
## output

```
hello test ok !
pubKey = 079b9296a00a2b655787fa90e66ec3cde4bf1c8c
rKey = 079b9296a00a2b655787fa90e66ec3cde4bf1c8c
aresult = 1
```
