
 ```kotlin
	val modulus = "v3eWNX1b3fTdROHikb_pi24r3oX-CrOKzl5xPbLxvpUt1etvhEJvnC2ncxr5g9LwrIyeL19m0fM1GUEojexqwHJVG-SEMWK6ay2rVhBDsH4QuCSHevUuSx7ACdGcUwvLOkzLL2vwtJt6-RTkZfraSWzz1eSsCdLeZawV0UPpNztq4lWyOJEcJmWcGR0xLW_79GOcnZmDNnh-wkCAqLMir5ywmVUAo1YDpI7cn5LFNp3O8I39BDY6gtHtWvvcW0O97LGs3x56uW_2cH4jzfVT3rt5aETJVSaKJDEJ_MVSEP-RRzkyogD1va1fShxhpBnpIBR6yotV3bWiWHU9Mc1V1Q"  
	val exponent = "AQAB"  
	val modulusByte: ByteArray = Base64.getUrlDecoder().decode(modulus)  
  
	val modulusAsBigInt = BigInteger(1, modulusByte)  
	val exponentByte: ByteArray = Base64.getUrlDecoder().decode(exponent)  
	val exponentAsBigInt = BigInteger(1, exponentByte)  
  
	val spec = RSAPublicKeySpec(modulusAsBigInt, exponentAsBigInt)  
	val factory = KeyFactory.getInstance("RSA")  
	val pub = factory.generatePublic(spec)  
	println(Base64.getEncoder().encodeToString(pub.encoded))
```

```java
String modulus = "v3eWNX1b3fTdROHikb_pi24r3oX-CrOKzl5xPbLxvpUt1etvhEJvnC2ncxr5g9LwrIyeL19m0fM1GUEojexqwHJVG-SEMWK6ay2rVhBDsH4QuCSHevUuSx7ACdGcUwvLOkzLL2vwtJt6-RTkZfraSWzz1eSsCdLeZawV0UPpNztq4lWyOJEcJmWcGR0xLW_79GOcnZmDNnh-wkCAqLMir5ywmVUAo1YDpI7cn5LFNp3O8I39BDY6gtHtWvvcW0O97LGs3x56uW_2cH4jzfVT3rt5aETJVSaKJDEJ_MVSEP-RRzkyogD1va1fShxhpBnpIBR6yotV3bWiWHU9Mc1V1Q";
  String exponent = "AQAB";
  byte[] modulusByte = Base64.getUrlDecoder().decode(modulus);

  BigInteger modulusAsBigInt = new BigInteger(1, modulusByte);
  byte[] exponentByte = Base64.getUrlDecoder().decode(exponent);
  BigInteger exponentAsBigInt = new BigInteger(1, exponentByte);

  RSAPublicKeySpec spec = new RSAPublicKeySpec(modulusAsBigInt, exponentAsBigInt);
  KeyFactory factory = KeyFactory.getInstance("RSA");
  PublicKey pub = factory.generatePublic(spec);
```