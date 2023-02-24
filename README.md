# 
import discord
#pip install discord.py-self

nonolist = [
    000000000000000000, # the ids of the ignored users, also here you need to add the account id that is used as a selfbot
    000000000000000000,
    000000000000000000,
]

notification_channel = 000000000000000000

class MyClient(discord.Client):
    async def on_ready(self):
        print('Logged on as', self.user)

    async def on_message(self, message):
        if message.author.id not in nonolist:
            with open('message_content.txt', 'a', encoding='utf-8') as file:
                file.write(f"{message.id}~{message.content}~{message.guild.id}~{message.author.id}\n")

    async def on_raw_message_delete(self, payload):
        message_id = payload.message_id
        with open('message_content.txt', 'r', encoding='utf-8') as file:
            for line in file:
                if line.startswith(str(message_id)):
                    deleted_message_content = line.split('~', 4)[1].strip()
                    deleted_message_guild = line.split('~', 4)[2].strip()
                    deleted_message_author = line.split('~', 4)[3].strip()
                    chn = client.get_channel(notification_channel)
                    guild = client.get_guild(int(deleted_message_guild))
                    author = client.get_user(int(deleted_message_author))
                    await chn.send(f"Message: {deleted_message_content}\nGuild: {guild.name}\nAuthor: {author.mention}")
                    break

client = MyClient()
client.run('')
