const Disocrd = require('discord.js')
const db = require('quick.db')
const Canvacord = require('canvacord')
const canvacord = require('canvacord')
const canvas = new Canvacord()

    module.exports = {
        name: 'rank',
        aliases: 'level',
        run: async (client, message, args) => {

xp(message)
        if (message.content.startsWith(`${PREFIX}rank`)) {
            let user = message.mentions.users.first() || message.author
            let level = db.get(`guild_${message.author.id}_level_${user.id}`)  || 0
            level = level.toString()
            let xp = db.get(`guild_${message.guild.id}_xp_${user.id}`) || 0
            let xpNeeded = level * 500 + 500
            let every = db
                .all()
                .filter(i => i.ID.startsWith(`guild_${message.guild.id}_xptotal_`))
                .sort((a, b) => b.data - a.data)
            let rank = every.map(x => x.ID).indexOf(`guild_${message.guild.id}_xptotal_${user.id}`) + 1
            rank = rank.toString()
            //  let image = await Canvacord.rank({
            //    username: user.username,
            //    discrim: user.discriminator,
            //    status: user.presence.status,
            //    currentXP: xp.toString(),
            //    neededXP: xpNeeded.toString(),
            //    rank,
            //    level,
            //    avatarURL: user.displayAvatarURL({ format: "png" }),
            //    color: "white"
            //})

            const card = new canvacord.Rank()
                .setUser(user.username)
                .setDiscriminator(user.discriminator)
                .setRank(rank)
                .setLevel(level)
                .setCurrentXP(exp)
                .setRequiredXP(neededXP)
                .setStatus(user.presence.status)
                .setAvatar(user.displayAvatarURL({ format: 'png', size: 1024 }))

                const img = await card.build();

            return message.channel.send(new Discord.MessageAttachemnet(image, "rank.png"))

        }

function xp(message) {
    if (message.content.startsWith(PREFIX)) return
    const randomNumber = Math.floor(Math.random() * 10)  + 15
    db.add(`guild_${message.guild.id}_xp_${message.author.id}`, randomNumber)
    db.add(`guild_${message.guild.id}_xptotal_${message.guild.id}`, randomNumber)
    var level = db.get(`guild_${message.guild.id}_level_${message.author.id}`) || 1 
    var xp = db.get(`guild_${message.guild.id}_xp_${message.author.id}`)
    var xpNeeded = level * 500
    if (xpNeeded < xp) {
        var newLevel = db.add(`guild_${message.guild.id}_level_${message.author.id}`, 1)
        db.subtract(`guild_${message.guild.id}_xp_${message.author.id}`, xpNeeded)
        message.channel.send(`${message.author}, Você upou para o Level ${newLevel}`)

            }
        }
    }
}
