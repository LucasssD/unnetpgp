UNNetPGP is Objective-C wrapper for [NetPGP](http://www.netpgp.com) for iOS.

**About**

The PGP solution you've been looking for is here. Low level C based api with Objective-C wrapper around it is all you need to encrypt and decrypt PGP messages. Based on [NetPGP](http://www.netpgp.com), a standards-compliant library and suite of utilities providing digital signature and verification functionality, as well as data encryption and decryption, using RSA and DSA/Elgamal keys.

**Known Issues**

Due to an unsolved crash when using the SHA-256 algorithm, this library [currently uses](https://github.com/upnext/unnetpgp/blob/master/netpgp/UNNetPGP.m#L584) the SHA-1 hash, [which is known to be weak.](http://www.apache.org/dev/release-signing.html#sha1) Patches to fix the crash would be greatly appreciated!

**Installation**

This package is intended to be used with [CocoaPods](http://cocoapods.org) to satisfy OpenSSL dependency.

* With [CocoaPods](http://cocoapods.org)

Add this to you `Podfile`:

	pod 'UNNetPGP', :podspec => 'https://raw.github.com/krzyzanowskim/unnetpgp/master/UNNetPGP.podspec'
 
* Without CocoaPods

Something with Source Trees should do the trick but haven't tested. Pull request welcome.


**Usage**

Initialize and setup

    UNNetPGP *pgp = [[UNNetPGP alloc] initWithUserId:@"marcin.krzyzanowski@gmail.com"];
    pgp.password = @"secret1234";
    pgp.armored  = YES

Optionally you can specify ringfiles out of home directory

    pgp.publicKeyRingPath = [[self documentsDirectory] stringByAppendingPathComponent:@"pubring.gpg"];
    pgp.secretKeyRingPath = [[self documentsDirectory] stringByAppendingPathComponent:@"secring.gpg"];

Lets define filenames. Caution: file extension is important for some files! (`.gpg`,`.asc`)

    NSString *plaintextFile = [myDir stringByAppendingPathComponent:@"plain.txt"];
    NSString *encryptedFile = [myDir stringByAppendingPathComponent:@"plain.txt.gpg"];
    NSString *decryptedFile = [myDir stringByAppendingPathComponent:@"plain.decoded.txt"];
    NSString *signatureFile = [myDir stringByAppendingPathComponent:@"plain.txt.asc"];

Encrypt file

    BOOL result = [pgp encryptFileAtPath:plainFilePath toFileAtPath:encryptedFilePath options:UNEncryptOptionNone];
    NSLog(@"encryptedFilePath = %@",@(result));

Decrypt file

    BOOL result = [pgp decryptFileAtPath:encryptedFilePath toFileAtPath:decryptedFilePath];
    NSLog(@"decryptFileAtPath = %@",@(result));

Generate new key (and save in keyring)

    BOOL success = [pgp generateKey:1024];

Create file signature    

    BOOL success = [pgp signFileAtPath:plaintextFile writeToFile:signatureFile detached:YES];
    
Verify file signature. Caution: there is assumption that signed file exists in the same directory.

	BOOL success = [pgp verifyFileAtPath:signatureFile];


**Authors**

[Marcin Krzyżanowski](https://twitter.com/krzyzanowskim)

