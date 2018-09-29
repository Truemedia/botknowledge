# Bot Knowledge
A list of resources for developing and integrating bots for many different use cases

## Resources
- [Comparing chatbot frameworks](https://docs.google.com/spreadsheets/d/1RgG-dRS42EHlG7QdJOTg2ZO587KutTTPeUfyxVKoIn8/edit#gid=0)

## Framework considerations
### Why should you use a framework? (LIMITATIONS)
- **Markup**, Speech assistancts like Alexa use SSML with some custom tags where as text chat services like slack use custom markdown. There isn't any library currently to write a template that works for all.
- **Service Api's**, There is no consolidated API for the services I want to use. Why write code more than once when it's doing the same thing on a different services?
- **Unintellgient**, Most chat bot architectures are still using conventions which are outdated and clunky, where as people want natural conversations with smart AI. Even voice assistants are lacking in terms of the leg work needed to recognise a wide spectrum of conversation scenarios which people would want to essentially to perform the same task.
- **Lack of personality**, It's nice to have a conversation with something that seems somewhat intellgent and has a personality you can connect with on a personal level. Generic and very stale language can make the experience boring and feel like a chore more than a nice thing to have.
- **Structure**, Most framework or api options leave very little in terms of ideas on how to structure your application. Improvising in this area can be daunting and quickly come back to bite you if you opt for the wrong strategy which essentially becomes a game of luckily fishing for useful libraries to get one thing done in the only way you can think how.
- **Amount of code**, To make any kind of skill with a reasonable amount of functionality and great user experience for multiple api's without a framework would be a massively daunting and time consuming experience. Why would you want that when boilerplate can save you from spaghetti code and hours of your life back?

### Plugins, Add adapters and skillsets to a bot instance
- Characters, Plugins for custom personalities and looks based on pre-built characters. Planned list includes:
  - Butler
  - Clippy
  - Hatsune Miku
- Adapters, Plugins for third party communication services. Planned list includes:
  - Alexa
  - Discord
  - Slack
  - WebRTC
- Skillsets, Plugins for bot custom functionality and features. Planned list includes:
  - Developer
  - Entertainment
  - Garden
  - Navigation
  - Wardrobe

### Extensions, Add data sources, quotes, and translations to compatable plugins
- Data sources, Extensions for data relevant to a specific skillset. Planned list includes:
  - Plant name database extract (Garden)
  - Countries (Navigation)
  - Clothing list (Wardrobe)
- Quotes. Planned list includes:
  - Movie quotes (Entertainment)
- Translations. Planned list includes:
  - Japanese (JA_JP)

### Context filtering, Alter response based on contextual scenarios
- Medium
  - Channel
  - Group chat
  - P2P/DM
- Association
  - group
  - channel
  - user
    - roles
    - individual
- Audience
  - Adult
    - NSFW
    - SFW
  - Kid
    - *AGE_RANGE*
- Transient
  - *EVENT_NAME*

### Libraries
- [Turndown](https://www.npmjs.com/package/turndown), Convert HTML (SSML) into MD files

### Framework structure
- Configuration, YAML files to set instance specific options
- Skillsets, Self contained modules containing code related to specific categories of activity (e.g News,Social)
  - DB, Folder containing database abstractions and underlying data structures
--- Schemas, Mongoose/JSONschema extended from base schemas for declaring data stores
---- UserData.js, Extended schema for holding all user generated content
---- Lexicon.js, Extended schema for holding dynamic phrases which are binded with intents
---- Intent.js, Extended schema for holding intents used to trigger skillset functionality
--- Seeds, Source data to fill the initial database instance
---- UserData.js,
---- Lexicon.js,
---- Intent.js,
--- Resolvers, Promise based functions used to resolve matched intents
--- Speech, Handlebars templates containing SSML used by resolvers as text or audio responses to user
-- Index, Entrypoint file used to connect skillset to main application runner

- /config
  - bot.yml
- /content
  /<skillsetName>
    UserData.js
    Lexicon.js
    Intentions.js
/skillsets
  /<skillsetName>
    index.js
    /db
      /fixtures
        UserData.js
        Lexicon.js
        Intentions.js
        /intentions
          <intentName>.js
      /schemas
        UserData.js
        Lexicon.js
        Intentions.js
    /slots
      <slotName>.json
    /resolvers
      /<resourceName>
        <resolverName>.js
        /quotes
          <quoteName>.txt
        /reactions
          /group
            /private
            /open
          /public
          /user
          /universal
            /speech
              <templateName>.ssml.hbs
            /display
              <templateName>.html.hbs
