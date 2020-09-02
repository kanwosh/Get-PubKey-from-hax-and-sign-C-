# Obtain PubKey from signature (C++)

## dependency：
     WaykiChain:3.2.1  
     download url: https://github.com/WaykiChain/WaykiChain.git

## test way
     using rpc

## test step
    1. Get pubKey named Key1 from privateKey
    2. Get pubKey named Key2 from hash and sign
    3. Compare Key1 and Key2

## source code
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

    bool recoverable = (rKey == pubkey ? true :false);
    cout << "recoverable = " << recoverable << endl;


    return true;
}

```
## result

```
hello test ok !
pubKey = 079b9296a00a2b655787fa90e66ec3cde4bf1c8c
rKey = 079b9296a00a2b655787fa90e66ec3cde4bf1c8c
recoverable = 1
```
