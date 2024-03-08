import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.typing = False
intents.presences = False

bot = commands.Bot(command_prefix='!', intents=intents)

@bot.event
async def on_ready():
    print(f'Logged in as {bot.user.name}')

@bot.command()
async def ticket(ctx):
    options = {
        'شكوى ضد اداري': {
            'admin_id': 1215758293516161225,
            'emoji_id': :question:
        },
        'استفسار': {
            'admin_id': 1215758323459035228,
            'emoji_id': :thumbsup_tone1:
        },
        'اعتراض على باند': {
            'admin_id': 1215758342803427358,
            'emoji_id': :crown:,
        }
    }

    embed = discord.Embed(title='Ticket Options', description='Please select the ticket type:', color=discord.Color.blue())

    options_text = ''
    for option, data in options.items():
        options_text += f'{data["emoji_id"]} - {option}\n'

    embed.add_field(name='Ticket Types', value=options_text, inline=False)
    embed.set_footer(text='React to this message to select a ticket type.')

    message = await ctx.send(embed=embed)

    for option in options.values():
        await message.add_reaction(option['emoji_id'])

@bot.event
async def on_raw_reaction_add(payload):
    if payload.member.bot:
        return

    channel = bot.get_channel(payload.channel_id)
    message = await channel.fetch_message(payload.message_id)
    user = payload.member

  options = {
        'شكوى ضد اداري': {
            'admin_id': 1215758293516161225,
            'emoji_id': :question:
        },
        'استفسار': {
            'admin_id': 1215758323459035228,
            'emoji_id': :thumbsup_tone1:
        },
        'اعتراض على باند': {
            'admin_id': 1215758342803427358,
            'emoji_id': :crown:,
        }
    }


    if message.author.bot and message.embeds:
        embed = message.embeds[0]
        if embed.title == 'Ticket Options':
            for option, data in options.items():
                if data['emoji_id'] == str(payload.emoji):
                    channel = await user.create_dm()
                    if option == 'إغلاق التذكرة':
                        if data['admin_id'] is None:
                            await channel.send('لقد تم إغلاق التذكرة.')
                            await message.delete()
                        else:
                            admin = bot.get_user(int(data['admin_id']))
                            await channel.send(f'تم إرسال طلب إغلاق التذكرة إلى {admin.mention}')
                            await message.delete()
                    elif option == 'استلام التذكرة':
                        if data['admin_id'] is None:
                            await channel.send('تم استلام التذكرة.')
                            await message.delete()
                        else:
                            admin = bot.get_user(int(data['admin_id']))
                            await channel.send(f'تم إرسال طلب استلام التذكرة إلى {admin.mention}')
                            await message.delete()

bot.run('MTIxNTc1MjQ5MzEzNzUzMTAwMA.GpwPq9.nuxDQVc2kc30526djAf3_UGaIX6NxTCpHSnOY8')
