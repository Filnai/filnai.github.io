<script>
    import { enhance } from "$app/forms";
    import { redirect } from "@sveltejs/kit";
    import "elizabot";
    import ElizaBot from "elizabot";
    import { } from "svelte/internal";
    let eliza = new ElizaBot();
    let chat = [{ user: "eliza", text: eliza.getInitial() }];
    async function write(message) {
      chat.push({user: "me", text: message})
      chat = chat
      // TODO: yeet in the new message
      // random delay for writing
      await new Promise((r) => setTimeout(r, 1000 + Math.random() * 1000));
        var reply = eliza.transform(message);
        chat.push({user: "eliza", text: reply});
      // TODO: write the text

     chat = chat
     
    }
  </script>
  
  
  <svelte:head>
    <link rel="stylesheet" href="/pico.min.css" />
    <style>
      nav {
        margin-left: 10%;
        margin-right: 10%;
      }
      
      .me {
        font-family: impact;
        background-color: #50C878;
        color: white;
      }

      .eliza {
        font-family: serif;
        background-color: #01796F;
        color: white;
      }
      
    </style>
  </svelte:head>
  
  <div class="container">
    <h1>TODO: Complete assignment</h1>
    <div class="scrollable">
      {#each chat as text, i}
      <article class="{chat[i].user}" >
        <span>
          {chat[i].text}
        </span>
      </article>
      {/each}
    </div>
    <form
      method="post"
      use:enhance={({ form, data, action, cancel }) => {
        /* https://kit.svelte.dev/docs/form-actions#progressive-enhancement */
        cancel(); //don't post anything to server
        const text = data.get("text");
        write(text);
  
        // TODO: reset the form using form.reset()
        form.reset()
      }}
    >
      <input type="text" name="text" />
    </form>
  </div>