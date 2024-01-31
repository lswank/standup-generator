# Standup Generator

Standup Generator is a wildly simple Vue.js application that uses the OpenAI API to generate structured standup check-in messages. They look something like this:

```
🚧 [PROD-123](link_to_ticket) ✅ Frontend adjustments for wallet interface in PROD-123 @alice
📅 [PROD-130](link_to_ticket) ⬇️ 💬 Sprint planning for scalability features in PROD-130
🔀 [PROD-115](link_to_ticket) ✅ Merged code for enhanced consensus algorithm in PROD-115
📅 [PROD-140](link_to_ticket) ⬇️ 🏢 Strategic meetings for coin integration in PROD-140
🚧 [PROD-132](link_to_ticket) ✅ Migrating coin's ledger data to new database architecture in PROD-132
🔍 [PROD-120](link_to_ticket) ✅ Created test cases for new smart contract templates in PROD-120
🚧 [PROD-124](link_to_ticket) ✅ Collaborating with design team for mobile app UI/UX improvements in PROD-124
📝 [PROD-130](link_to_ticket) ⬇️ Refining user stories for customer feedback mechanism in PROD-130
📅 [PROD-121](link_to_ticket) ⬇️ 🏢 One-on-one with manager for customer retention strategies in PROD-121
```

## Motivation

I wanted my standup check-in messages to be consistent, and I also wanted to automate the process of creating them. If I can automate something, make it pretty, make it fun, and look more polished than other people, I'm all for it.

But also, if I can build a tool that causes the teams I am on to look way more polished than other teams, I'm all for that, too.

Unfortunately, it's totally not acceptable for me to put work data through OpenAI's API, so I can't use this tool for my actual standup check-ins. (We've got Azure OpenAI so maybe this will serve as a pretty demo to make the case to tie into the Azure OpenAI API behind the curtain.)


## Getting Started Developing
To get started with this project, clone the repository and install the dependencies:

> npm install openai@^4.0.0

Then, you can start the development server with:

> npm run serve 

## Usage
To use the Standup Generator, you need to enter your OpenAI API key in the options. Then, enter some input to process and click the "Submit" button. The generated standup check-in message will appear in the output area.

## License
This project is licensed under the AGPL-3.0 License.

## Contributing
Contributions are welcome! Send me a PR!

## Contact
Open an issue, send us a pull request, or message me on Twitter/X at @LSwank.

