# Deprecated

See [https://www.nuget.org/packages/PInvoke.BCrypt/](https://www.nuget.org/packages/PInvoke.BCrypt/) for details.

# Example

```
private static string _decryptWithKey(byte[] message, byte[] key)
{
    const int MAC_BIT_SIZE   = 128;
    const int NONCE_BIT_SIZE = 96;

    using (var cipherStream = new MemoryStream(message))
    using (var cipherReader = new BinaryReader(cipherStream))
    {
        var nonce      = cipherReader.ReadBytes(NONCE_BIT_SIZE / 8);
        var cipher     = new GcmBlockCipher(new AesEngine());
        var parameters = new AeadParameters(new KeyParameter(key), MAC_BIT_SIZE, nonce);
        cipher.Init(false, parameters);

        var cipherText = cipherReader.ReadBytes(message.Length);
        var plainText  = new byte[cipher.GetOutputSize(cipherText.Length)];

        var len = cipher.ProcessBytes(cipherText, 0, cipherText.Length, plainText, 0);
        cipher.DoFinal(plainText, len);

        return Encoding.UTF8.GetString(plainText);
    }
}
```
