- 👋 Hi, I’m @ofaaoa
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

from pyrogram import Client, filters, idle, types
import re


app = Client(
    "name",
    29318521,
    "0db9b7d5728fbc65fbe43dd70717a042",
    bot_token="6174386027:AAHIJjdsLWKRjkGpZQyou3xXIm9CIw4qsrQ",
    in_memory=True
)

app.start()
start_string = '''
مرحبًا بك {}

انا بوت نسخ محتوى من اي قنوات مقيجوة المحتوى 

فقط ارسل رابط المنشور من قناة عامة 

مثال: https://t.me/telegram/10
'''
@app.on_message(filters.text & filters.private)
async def on_text(c: Client, m: types.Message):
    text = m.text
    if re.findall("((www\.|http://|https://)(www\.)*.*?(?=(www\.|http://|https://|$)))", text):
        url = re.findall("((www\.|http://|https://)(www\.)*.*?(?=(www\.|http://|https://|$)))", text)[0][0]
        print(url)
        if "t.me/" in url:
            if "c/" in url:
                return await m.reply("ارسل ربط من قناة عامه", quote=True)
            else:
                channel = url.split("t.me/")[1].split("/")[0]
                msg_id = int(url.split("t.me/")[1].split("/")[1])
                reply = await m.reply("انتظر ....", quote=True)
                msg = await c.get_messages(channel, msg_id)
                await reply.delete()
                if not msg.chat.has_protected_content:
                    return await m.reply("المنشور غير مقيد", quote=True)
                if msg.text:
                    return await m.reply(msg.text.html, quote=True, reply_markup=msg.reply_markup)
                if msg.media_group_id:
                    return await c.copy_media_group(m.chat.id, msg.chat.id, msg.id)
                if msg.media:
                    return await msg.copy(m.chat.id, reply_markup=msg.reply_markup)
        else:
            return await m.reply("لازم رابط منشور من قناة", quote=True)
    else:
        return await m.reply(start_string.format(m.from_user.mention))

print(app.me.username)
idle()

<!---
ofaaoa/ofaaoa is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
