<template>
  <div id="app">
    <h1>
        Standup Check-in Generator
    </h1>
    <p>
      Have you ever wanted to harness the mind-boggling powers of artificial intelligence to transform your daily drudgery into a symphony of automated bliss? No? Well, here's your chance to jump on the bandwagon anyway because who needs mundane human efficiency when you can have the cosmic ballet of technology at your fingertips!</p>

<p>Introducing the world's most unnecessarily over-engineered, AI-powered status report generator! Gone are the days of typing out your daily tasks like some sort of Dickensian clerk. With this marvel of modern wizardry, you can now summon the digital spirits of Silicon Valley to conjure up your status reports by entering in some word vomit.</p>

<p>Note: Requires your own OpenAI API Key. If you're logged in, you can get one <a href="https://platform.openai.com/api-keys">here</a>. Also, don't use this in secure environments. Data goes to OpenAI, not to me.
    </p>
    <div class="flex-container">
      <button 
          @click="showOptions = !showOptions">
          Toggle API Key Entry
      </button>
      <div 
          v-if="showOptions">
          <input 
              type="text" 
              v-model="openaiKey" 
              placeholder="Enter OpenAI API Key" 
              @blur="saveOpenAIKey">
      </div>
      <input 
          type="text" 
          v-model="userInput" 
          placeholder="Vomit your status into here. Prefer dictation when possible.">
      <button 
          @click="processInput">
          Submit
      </button>
      <textarea 
          v-model="output" 
          placeholder="Magical output will appear here" 
          readonly>
      </textarea>
    </div>
    <div class="social-links">
      
    </div>
    <p>Your PRs are welcome <a href="https://github.com/lswank/standup-generator" target="_blank">
        <github-icon />
      </a>, or leave me a comment on <a href="https://twitter.com/LSwank" target="_blank">
        <twitter-icon />
      Twitter</a> </p>
  </div>
</template>

<script>
import { OpenAI } from 'openai';
import { AkTwitterFill, AkGithubFill } from "@kalimahapps/vue-icons";

export default {
  components: {
    'twitter-icon': AkTwitterFill,
    'github-icon': AkGithubFill
  },
  data() {
    return {
      openai: null,
      openaiKey: localStorage.getItem('openaiKey') || '',
      userInput: '',
      output: '',
      showOptions: false,
    };
  },
  created() {
    if (this.openaiKey) {
        this.openai = new OpenAI({ apiKey: this.openaiKey, dangerouslyAllowBrowser: true });
      }
  },
  methods: {
    saveOpenAIKey() {
      localStorage.setItem('openaiKey', this.openaiKey);
      if (this.openaiKey) {
        this.openai = new OpenAI({ apiKey: this.openaiKey });
      }
    },
    async processInput() {
      if (!this.openaiKey) {
        alert('Please enter your OpenAI API key in options.');
        return;
      }
      if (!this.userInput) {
        alert('Please enter some input to process.');
        return;
      }
      if (!this.openai) {
        alert('OpenAI is not initialized. Please save your OpenAI API key again.');
        return;
      }
      this.output = 'Processing...';

      let prompt = `Generate a structured standup check-in message where each line represents a specific task or activity, following a predefined format. Each line combines emojis, links, and text to convey the task type, priority, status, and other relevant details clearly and concisely.

        ### Output Format Template:

        <Activity Emoji> <JIRA Link (if any)> <Priority Emoji (if any)> <Blocker Emoji (if any)> <Status Emoji> <Details> <Callouts (if any)>


        ### Emoji Legend:
        - 📅: Meetings
        - 🔧: Code PRs Produced
        - 👀: Code PRs Reviewed
        - 🔀: PRs Merged
        - 💬: Discussions
        - 🏢: Corporate Meetings
        - ⬆️: High Priority
        - ⬇️: Low Priority
        - 🚫: Blocker
        - 📝: Planned
        - 🚧: In Progress
        - 🔍: In Review
        - ✅: Completed

        ### Examples:

        🔧 [JIRA-123](link_to_ticket) ⬆️ 🚧 Refactoring module X @alice @bob
        💬 ⬇️ ✅ Discussed project scope with client @client

        ### Pseudocode:
        for each task in standup_check_in:
            line = ""
            line += get_activity_emoji(task.type) // e.g., 🔧 for 'Code PRs Produced'
            
            if task.jira_ticket:
                line += create_markdown_link(task.jira_ticket) // e.g., [JIRA-123](link_to_ticket)
            
            if task.priority:
                line += get_priority_emoji(task.priority) // e.g., ⬆️ for 'High Priority'
            
            if task.is_blocker:
                line += get_blocker_emoji() // e.g., 🚫
            
            line += get_status_emoji(task.status) // e.g., 🚧 for 'In Progress'
            line += " " + task.details // e.g., 'Refactoring module X'
            
            if task.callouts:
                line += " " + create_callouts(task.callouts) // e.g., '@alice @bob'
            
            print(line)

        In this format, each line of the standup check-in message is constructed by sequentially appending the appropriate emoji, JIRA link, priority, blocker, status, and additional details. The order is strictly followed to maintain consistency and readability.
              `

      try {
        const response = await this.openai.chat.completions.create({
          model: 'gpt-3.5-turbo',
          messages: [
            {
              role: 'system',
              content: prompt
            },
            {
              role: 'user',
              content: this.userInput
            }
          ]
        });
        this.output = response.choices[0].message.content;
      } catch (error) {
        console.error(error);
        this.output = 'An error occurred while processing your input.';
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.flex-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.flex-container > * {
  margin: 20px 0;
}

input, textarea {
  width: 80%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

textarea {
  height: 150px;
  resize: vertical;
}
</style>
