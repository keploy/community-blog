---
title: "🧪 Testing with ChatGPT : Epic Wins and Fails"
seoTitle: "Testing with ChatGPT 🤖: Epic Wins and Fails 🎭"
seoDescription: "Discover the ins and outs of ChatGPT testing in this developer-friendly guide. Uncover its strengths, challenges, and how it complements your testing strate"
datePublished: Thu Aug 17 2023 16:48:12 GMT+0000 (Coordinated Universal Time)
cuid: cllfeaxvc000209mq9mut6a2w
slug: testing-with-chatgpt-epic-wins-and-fails-api-testing
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1692288344722/20293e51-a1b8-4a58-bf42-f096d90d7172.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1692290747997/2ec88488-3570-477b-b1d0-a3139d6a3d58.png
tags: testing, test-automation, testing-tool, generative-ai, chatgpt

---

Hey devs, 🖐️ let's talk about a shiny new tool that's shaking up testing – ChatGPT 🚀. It's like your sidekick, but is it the superhero it claims to be? Grab some popcorn and let's break down how this AI buddy can rock the testing boat that might make you go, "Aha!" or "Oops!"

**Seeing ChatGPT in Action:**

ChatGPT, the AI wizard powered by Generative Pre-trained Transformer, has some tricks up its sleeve, but hold on tight – there's more to the story! 📜

**Upsides in Real-World Testing:**

1. **Quick Test Ideas:** Let's say you're building a music app. ChatGPT suggests:
    
    ```python
    # ChatGPT's Idea 🎵
    test_song_playback():
        open_app()
        search_song("GPT Groove")
        play_song()
        check_sound_output()
    ```
    
    **Plot Twist:** But it's not aware of a silent mode bug, so the sound check might end in deafening silence! 👊🏻
    
2. **Exploring Uncharted Territory:** Imagine you're working on a map app. ChatGPT pitches:
    
    ```python
    # ChatGPT's Idea 🗺️
    test_directions():
        input_start_location("Mystic Street")
        input_end_location("Rainbow Avenue")
        get_directions()
        verify_route_on_map()
    ```
    
    **Drama Unfolds:** Whoops, it doesn't account for a detour or GPS glitch that might pop up. 🤖
    
3. **Rare User Simulator:** ChatGPT offers help for your chat app:
    
    ```python
    # ChatGPT's Idea 💬
    test_user_chat():
        send_message("Hey Keploy!")
        wait_for_response()
        verify_received_message("Hello, Developer!")
    ```
    
    **Drama Unfolds:** But sometimes, the AI might reply with "Greetings, toaster!" 🍞🤖
    

**Facing the Hilarious Challenges:**

1. **Half-Baked Code:** Here's the deal - the code ChatGPT creates is often like a half-cooked meal – you gotta add your seasoning to make it truly appetizing (and functional). Back to the music app:
    
    ```python
    # ChatGPT's Idea 🎵
    test_song_playback():
        open_app()
        search_song("GPT Groove")
        play_song()
        # Oops, missed checking song volume
        check_sound_output()
    ```
    
2. **Jigsaw Integration:** Plugging ChatGPT into your testing flow isn't always plug-and-play. The generated code can't always be copy-pasted right into your project. It's like solving a jigsaw puzzle, connecting the ChatGPT dots to your real-world code. Your map app adventure:
    
    ```python
    # ChatGPT's Idea 🗺️
    test_directions():
        input_start_location("Mystic Street")
        input_end_location("Rainbow Avenue")
        # Forgot about unexpected traffic rerouting
        verify_route_on_map()
    ```
    
3. **Context Clues:** Sometimes, ChatGPT might miss the point. It struggles with context sometimes, generating code that's a bit off. You've got to be the context detective to make sure it's on track. ChatGPT tries its hand at social media:
    
    ```python
    # ChatGPT's Idea 💬
    test_user_chat():
        send_message("Hey Test Generator!")
        wait_for_response()
        # Missed the context check
        verify_received_message("Hello Keploy!")
    ```
    
    **Drama Unfolds:** Sometimes, it's like expecting a high-five and getting a handshake! 🤝
    

**Walking the Line: Balancing Pros and Cons**

Remember, ChatGPT isn't a one-stop solution. But it can be a trusty sidekick in your testing journey.

**Battle-Tested Tips:**

1. **Check Before You Leap:** Before you hit that run button, double-check ChatGPT's code. Add the missing ingredients and make it rock!
    
2. **Hybrid Hack:** Mix and match! Think of ChatGPT as your brainstorming buddy:
    
    ```python
    # Traditional test case
    test_song_playback():
        open_app()
        search_song("GPT Groove")
        play_song()
        check_sound_output()
    ```
    
    **P.S.:** ChatGPT sparks the idea; traditional tests catch the fire.🔥
    
3. **Targeted Tasks:** Pick your battles wisely. Use ChatGPT for those brain-twisting test scenarios that make your brain ache. Let it tackle those while you focus on the rest. For UI testing:
    
    ```python
    # ChatGPT's Idea 💬
    test_user_chat():
        send_message("Hey ChatGPT!")
        wait_for_response()
        verify_received_message("Hello, human!")
    ```
    
    **Drama Unfolds:** Just make sure you're checking those AI responses – sometimes, it’s a bit of a comedy show!
    

*Grab your developer cap and fasten your seatbelt! We're about to embark on an exhilarating journey into the heart of advanced API testing. Hold onto your code – it's about to get seriously exciting.* 🚀

### **🎯 Setting the Stage: The API Schema**

```python
# OpenAPI Schema Snippet
paths:
  /products:
    get:
      summary: Get a list of products
      responses:
        '200':
          description: Successful response
  /orders:
    post:
      summary: Place an order
      requestBody:
        $ref: '#/components/requestBodies/OrderRequestBody'
      responses:
        '200':
          description: Order successfully placed
```

### **🤖 ChatGPT: A Buddy with Limits of API Testing**

Enter ChatGPT, your trusty AI companion. It's superb at the basics – generating test cases, and understanding scenarios. But wait, what's that? 🤔 When you toss in intricate situations like caching issues or error resilience, it hesitates. Like a superhero with one weakness, it can't handle every twist in the plot.

Let's paint a picture. ChatGPT might stumble when you unleash these end-to-end scenarios:

1. 🏎️ **Concurrency Crunch:** Orders flying in all directions – how does your API handle the chaos?
    
2. 📦 **Cache Quest:** Caching product lists, but orders mix it up – can your data stay cool?
    
3. 🚨 **Error Embrace:** When payment gateways fail, will your API stand tall or stumble?
    
4. 🚦 **Rate Race:** Excessive requests are knocking – can you keep up the pace?
    
5. 🍔 **Quantity Quirk:** When orders defy logic with odd quantities, will your API play along?
    

### **🚀 Buckle Up for Resilient Software!**

In the ever-evolving landscape of software testing, we've harnessed innovation to address challenges left uncharted by AI models like ChatGPT. Presenting [Keploy](https://www.keploy.io/) 🐰, an open-source solution designed to address the limitations experienced with AI models like ChatGPT.

Born out of the need for thorough API testing, Keploy excels at crafting scenarios that mimic real API interactions. Stay tuned for more insights on how Keploy is reshaping API testing in our upcoming blog. 💪

Imagine you're crafting an epic e-commerce platform. You've got the OpenAPI schema, and you're ready to roll. 🛒

**Bottom Line: ChatGPT's Your Wingman**

So, should you hop on the ChatGPT testing train? Absolutely! Just know that it's not a silver bullet. With a mix of creativity and a developer's touch, it can help you unearth test gems and shake hands with new perspectives. 🎭 It's like a sidekick in a buddy-cop movie – not the star, but useful!

Found our testing journey relatable? 🤪 Tap that like button and tell us your funny coding moments too! ❤️