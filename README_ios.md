

## Bundle and sign

```
.github/workflows/ios_build.yml
```

Download: https://developer.apple.com/account/resources/profiles/list
```
ios/match_Development_apphowsinmobile.mobileprovision
```

Repository secret: IOS_MOBILE_PROVISION_BASE64
```
openssl base64 < ios/match_Development_apphowsinmobile.mobileprovision | tr -d '\n' | tee ios/match_Development_apphowsinmobile.base64.txt
```


Download: https://developer.apple.com/account/resources/certificates/list
```
ios/apple_development_gailen.cer
```

+ save as p12 (requires original cert used on requesting this one): https://stackoverflow.com/questions/9418661/how-to-create-p12-certificate-for-ios-distribution
```
ios/apple_development_q1w2e3r4t5.p12
```

Repository secret: IOS_P12_BASE64
```
openssl base64 < ios/apple_development_q1w2e3r4t5.p12 | tr -d '\n' | tee ios/apple_development_q1w2e3r4t5.p12.base64.txt
```
Repository secret: IOS_CERTIFICATE_PASSWORD
```
q1w2...
```
Repository secret: IOS_TEAM_ID
your apple team id https://developer.apple.com/account/#!/membership/
```
D3S3NKR5T5
```


#upload

APPSTORE_ISSUER_ID : Go to Users and Access > API Keys, the issuer id is something like 598542-36fe-1a63-e073-0824d0166672a
APPSTORE_API_KEY_ID: The Key ID for AppStore Connect API.
APPSTORE_API_PRIVATE_KEY: The PKCS8 format Private Key for AppStore Connect API. The content of the file AuthKey_xxxxxx.p8 generated






GH secrets (Github project home page > Project Setting > Secrets > Actions > new repository secret)

- ANDROID_SIGNING_KEY: The base64 encoded signing key used to sign your app
```
openssl base64 < android/app/upload-to-play-store.keystore  | tr -d '\n' | tee android/app/upload-to-play-store.keystore.base64.txt
```

- ANDROID_ALIAS: google-play-store-key
- ANDROID_KEY_STORE_PASSWORD: 4ndr01d
- ANDROID_KEY_PASSWORD: 4ndr01d


## Release versions to github
Better npm publish
```
npm i np
npm i react-native-version
```

package.json
```
    scripts {
        ...
        "np": "np --no-publish",
        "postversion": "react-native-version"
    }

    "repository": {
        "type": "git",
        "url": "git+https://github.com/jercilla/react-native-release.git"
     }

```

>We added react-native-version command as a postversion script because np can't >update ios and android versions. react-native-version will help sync package.json >version with android and ios as well as increment the build number automatically. 

```
npm run np
```


