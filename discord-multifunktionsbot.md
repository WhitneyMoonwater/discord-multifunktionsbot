# my-first-project
if (command == "giveaway") {
    // !giveaway {time s/m/d} {item}
    const messageArray = message.content.split(" ");
    if (!message.member.hasPermission(["ADMINISTRATOR"])) return message.channel.send("Du hast keine Rechte, um ein Giveaway zu starten!")
    var item = "";
    var time;
    var winnerCount;
    for (var i = 1; i < args.length; i++) {
      item += (args[i] + " ");
    }
    time = args[0];
    if (!time) {
      return message.channel.send(`Diese Dauer ist ungültig!`);
    }
    if (!item) {
      item = "No title"
    }
    var embed = new Discord.MessageEmbed();
    embed.setColor(0x3333ff);
    embed.setTitle("New Giveaway !");
    embed.setDescription("**" + item + "**");
    embed.addField(`Duration : `, ms(ms(time), {
      long: true
    }), true);
    embed.setFooter("Reagiere mit 🎉  auf diese Nachricht, um teilzunehmen!");
    var embedSent = await message.channel.send(embed);
    embedSent.react("🎉");

    setTimeout(async () => {
      try{
        const peopleReactedBot = await embedSent.reactions.cache.get("🎉").users.fetch();
        var peopleReacted = peopleReactedBot.array().filter(u => u.id !== client.user.id);
      }catch(e){
        return message.channel.send(`Ein Fehler ist passiert während dem Entwurf des Giveaways **${item}** : `+"`"+e+"`")
      }
      var winner;

      if (peopleReacted.length <= 0) {
        return message.channel.send(`Nicht genügend Teilnehmer, um die Auslosung des Gewinnspiels **${item}** durchzuführen :(`);
      } else {
        var index = Math.floor(Math.random() * peopleReacted.length);
        winner = peopleReacted[index];
      }
      if (!winner) {
        message.channel.send(`Ein Fehler ist passiert während dem Entwurf des Giveaways **${item}**`);
      } else {
        console.log(`Giveaway ${item} won by ${winner.toString()}`)
        message.channel.send(`🎉 **${winner.toString()}** hat gewonnen **${item}** ! Herzlichen Glückwunsch ! 🎉 
        Melde dich bei einem Admin, um deine Belohnung abzuholen!`);
      }
    }, ms(time));
}
