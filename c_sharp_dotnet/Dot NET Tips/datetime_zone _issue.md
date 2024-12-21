## Solving zone datetime issue⚡

C# converts UTC time to local time with the following method:

𝚍𝚊𝚝𝚎𝚝𝚒𝚖𝚎.𝚃𝚘𝙻𝚘𝚌𝚊𝚕𝚃𝚒𝚖𝚎()

This works well to what you expect when your server is located on your local machine.

This might not result to the expected time when your code is deployed to a remote server.

ToLocalTime() converts UTC time to where the server is located.

So 𝚍𝚊𝚝𝚎𝚝𝚒𝚖𝚎.𝚃𝚘𝙻𝚘𝚌𝚊𝚕𝚃𝚒𝚖𝚎() can not be shared differently to different zones.

With C# 𝐓𝐢𝐦𝐞𝐙𝐨𝐧𝐞𝐈𝐧𝐟𝐨 class, we are able to share the local time of a specific zone.

𝐓𝐢𝐦𝐞𝐙𝐨𝐧𝐞𝐈𝐧𝐟𝐨 is great in building applications used internationally.

By getting the Time zone id of a specific zone, we can get the datetime.

What issue have you solved with time zone info? 

![image](https://github.com/user-attachments/assets/26561812-cc81-4cc5-8b59-4b3602db9e19)
