## Simple JS Discord Bot With Redstone API

A step-by-step guide on how to use RedStone API for your Discord Bot:

>[!WARNING]
>Before create project, make sure you already install [node.js](https://nodejs.org/en)

Step 1: Setting Up the Project.

1. Create project folder. Then first file, name it `index.js`.
2. Import **discord.js** library. Use this command in terminal `npm install discord.js`
3. Import **dotenv** library for configuration your bot. Use this command in terminal `npm install dotenv`
4. Import **redstone-api** library for using redstone api. Use this command in terminal `npm install redstone-api`
5. Create file and name it `.env` (This file will be used to configure the bot)
6. Create file and name it `register-commands.js` (This file will be used to create your own commands for bot)

Step 2: Bot settings.

1. Go to [Discord Application](https://discord.com/developers/applications) and press button **New Application** to create Bot.
2. In **OAuth2** → General ![image](https://github.com/CryptoFenomen/SimpleDiscordBotWithRedstoneApi/assets/156483400/87e47d2c-b77d-4752-8498-a3521d066a1c)
   
3. In **OAuth2** → URL Generator ![image](https://github.com/CryptoFenomen/SimpleDiscordBotWithRedstoneApi/assets/156483400/9206a9a9-40cd-4a21-b820-7f75a2e0d440)
   
5. In **Bot** ![image](https://github.com/CryptoFenomen/SimpleDiscordBotWithRedstoneApi/assets/156483400/588ce47e-6db9-4166-86a6-dd227adba4e9)

6. Save Changes, and copy **GENERATED URL** ![image](https://github.com/CryptoFenomen/SimpleDiscordBotWithRedstoneApi/assets/156483400/740af99f-ad04-4ef0-9b21-70ea71484fa1)
7. Add your bot, to your own server, just put link what you copy before.

Step 3: File setting.

>[!Warning]
> You must need turn ON Developer Mode (Setting → Advanced Setting → Developer Mode)

1. Go .env file and paste
```
TOKEN = #Discord Token. You can copy token from Discord Application (Bot Tab).
GUILD_ID = #Discord Server ID. Press right button on your server and copy id.
CLIENT_ID = #Press right button on your Bot, and copy id.
```
2. Setting index.js
```js
require('dotenv/config');
const {Client, EmbedBuilder} = require('discord.js');
const redstone = require('redstone-api');

const client = new Client({
    intents: ['Guilds', 'GuildMembers', 'GuildMessages', 'MessageContent'],
});


client.on('ready', () => {
    console.log('ready');
})
client.on('interactionCreate', (interaction) => {

  if (!interaction.isChatInputCommand()) return;
  if(interaction.commandName === 'price') {
     async function fetchPrices(){
      const prices = await redstone.getPrice(nametoken);
      interaction.reply(`Price of ${nametoken} = ${prices.value}`);
    }
    const nametoken = interaction.options.get('token').value;
    console.log(fetchPrices());
  }
  
  if(interaction.commandName === 'pricelist') {
    async function fetchListPrices(){
      const price = await redstone.getPrice(tokenname);
      const pBinance = price.source.binance.toFixed(1);
      const pBitGet = price.source.bitget.toFixed(1);
      const pBingx = price.source.bingx.toFixed(1);
      const pKucoin = price.source.kucoin.toFixed(1);
      const pOkx = price.source.okx.toFixed(1);
      const pGate = price.source.gate.toFixed(1);
      

      const embed = new EmbedBuilder()
    .setTitle(`List of price of ${tokenname} at services.`)
    .addFields({
      name: 'Binance',
      value: `${pBinance}`,
      inline: true,
    },
    {
      name: 'Bitget',
      value: `${pBitGet}`,
      inline: true,
    },
    {
      name: 'Bingx',
      value: `${pBingx}`,
      inline: true,
    },
    {
      name: 'OKX',
      value: `${pOkx}`,
      inline: true,
    },
    {
      name: 'Kucoin',
      value: `${pKucoin}`,
      inline: true,
    },
    {
      name: 'Gate',
      value: `${pGate}`,
      inline: true,
    });
    

      interaction.reply({embeds: [embed]});
    }
    const tokenname = interaction.options.get('token').value;
    console.log(fetchListPrices());
  }
})

client.login(process.env.TOKEN);
```
3. Setting register-commands.js
```js
require('dotenv').config();
const {REST, Routes, ApplicationCommandOptionType} = require('discord.js');

const commands = [
    {
        name: 'price',
        description: 'Give token price',
        options: [
            {
                name: 'token',
                description: 'Choose your available token here',
                type: ApplicationCommandOptionType.String,
                choices: [
                    {
                        name: 'BTC',
                        value: 'BTC',
                    },
                    {
                        name: 'ETH',
                        value: 'ETH',
                    },
                    {
                        name: 'AVAX',
                        value: 'AVAX',
                    },
                ],
                required: true,
            }
        ]
    },
    {
        name: 'pricelist',
        description: 'Send token list price',
        options: [
            {
                name: 'token',
                description: 'Choose your available token here',
                type: ApplicationCommandOptionType.String,
                choices: [
                    {
                        name: 'BTC',
                        value: 'BTC',
                    },
                    {
                        name: 'ETH',
                        value: 'ETH',
                    },
                    {
                        name: 'AVAX',
                        value: 'AVAX',
                    },
                ],
                required: true,
            }
        ],
        required: true,
    }
];
const rest = new REST({version: '10'}).setToken(process.env.TOKEN);

(async () => {
    try{
        console.log('Registering slash commands...');
        await rest.put(
            Routes.applicationGuildCommands(process.env.CLIENT_ID, process.env.GUILD_ID),
            {body: commands})

        console.log('Slash commands were registered successfully');
    }catch (error) {
        console.log(`There was an error: ${error}`);
    }
}) ();
```
Step 4: Testing.
1. When you have finished setting up, run this command `node register-commands.js` on your terminal. ![image](https://github.com/CryptoFenomen/SimpleDiscordBotWithRedstoneApi/assets/156483400/289b5c17-bf3a-4277-a232-5382e24d6452)

2. Run command `node .`

   ![image](https://github.com/CryptoFenomen/SimpleDiscordBotWithRedstoneApi/assets/156483400/c0d468c9-e20b-45da-9a69-18f79879db8d)
3. Enjoy

   ![image](https://github.com/CryptoFenomen/SimpleDiscordBotWithRedstoneApi/assets/156483400/f894340f-9399-48d5-b445-7d354782140f)

   ![image](https://github.com/CryptoFenomen/SimpleDiscordBotWithRedstoneApi/assets/156483400/46400531-d384-4a4a-b2d9-127a5ccbdbf9)
   
>[!Warning]
>When you try create your own command, after coding use this command `node register-commands.js`, because if you don't do that, bot can't see your commands at all.



