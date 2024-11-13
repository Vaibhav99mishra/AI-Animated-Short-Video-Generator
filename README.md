# AI-Animated-Short-Video-Generator


# **How to Build an AI Animated Short Video Generator using Next.js and Replicate's**

Are you ready to create your very own animated short videos with the magic of AI. In this blog, we'll show you how to build an AI animated short video generator using Next.js and Replicate's cutting-edge **`deforum_stable_diffusion`** model.

## **Introducing deforum_stable_diffusion Model**

The **`deforum_stable_diffusion`** model is your ticket to bringing prompts to life with captivating animations. It's all about infusing creativity into short videos. With this model, you can easily create small animated wonders with your prompts, making your ideas dance and twirl on the screen.

## **Prerequisites**

To follow along with this tutorial, you should have the following prerequisites:

1. Node.js and npm (Node Package Manager) installed on your machine.
2. Basic knowledge of JavaScript and React.
3. An active Replicate account and an API token. If you don't have one, you can sign up for an account on the [Replicate website](https://replicate.com/facebookresearch/musicgen).

## **Setting Up the Next.js Project**

Let's start by setting up a new Next.js project. Open your terminal and execute the following commands:

```bash

npx create-next-app ai-animator

✔ Would you like to use TypeScript with this project? … No
✔ Would you like to use ESLint with this project? … Yes
✔ Would you like to use Tailwind CSS with this project? … Yes
✔ Would you like to use `src/` directory with this project? … Yes
✔ Use App Router (recommended)? … No
✔ Would you like to customize the default import alias? … No
```

This will create a new Next.js project in a directory named **`ai-animator`**. Next, open the project in your preferred code editor.

```bash
cd ai-animator
```

## **Installing Dependencies**

Next, we need to install the required dependencies for our project. In the terminal, navigate to the project directory and run the following command:

```bash
npm install replicate
```

Now, create a new file called **`.env`** in the root of your project and add the following environment variables:

```sql
REPLICATE_API_TOKEN=<paste-your-token-here>
```

Retrieve your API token from your [Replicate account settings](https://replicate.com/account/api-tokens).

## **Creating the Backend**

Now, let's dive into the world of animation and build our AI video generator. Create a new file named **`video-generator.js`** inside the **`src/pages/api`** directory. This file will house the code for generating AI animated short videos.

```jsx
import Replicate from "replicate";

export default async function handler(req, res) {
  const replicate = new Replicate({
    auth: process.env.REPLICATE_API_TOKEN,
  });
  console.log(req.body.prompt);
  console.log(process.env.REPLICATE_API_TOKEN);
  try {
    const output = await replicate.run(
        "deforum/deforum_stable_diffusion:e22e77495f2fb83c34d5fae2ad8ab63c0a87b6b573b6208e1535b23b89ea66d6",
        {
          input: {
            max_frames: 100,
            animation_prompts: req.body.prompt,
          }
        }
      );
    // console.log(output);

    res.status(200).json({ video: output });
  } catch (error) {
    console.error("AI video generation failed:", error);
    res.status(500).json({ error: "AI video generation failed" });
  }
}
```

The code sets up a handler function to initialize the Replicate client using your API token. It then calls the **`run`** method to generate an animated short video using the **`deforum_stable_diffusion`** model, with a maximum of 100 frames based on the provided prompt.

## **Creating the Frontend**

To witness the magic of AI animation, let's create a frontend interface. Open the **`pages/index.js`** file and replace its content with the following code:

```jsx
import { useState } from "react";

export default function Home() {
  const [video, setVideo] = useState("");
  const [prompt, setPrompt] = useState("");
  const [isLoading, setIsLoading] = useState(false);

  const generateVideo = async () => {
    setIsLoading(true);

    try {
      const response = await fetch("/api/video-generator", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ prompt }),
      });

      const { video } = await response.json();
      setVideo(video);
    } catch (error) {
      console.error("Failed to generate video:", error);
    }

    setIsLoading(false);
  };

  return (
    <div className="flex flex-col items-center justify-center h-screen">
      <h1 className="text-3xl font-bold mb-6">AI Animated Video Generator</h1>
      <input
        type="text"
        value={prompt}
        onChange={(e) => setPrompt(e.target.value)}
        placeholder="Enter your animation prompt"
        className="px-4 py-2 text-black border border-gray-300 rounded-lg mb-4 w-80 text-center"
      />
      <button
        onClick={generateVideo}
        className="px-4 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600"
        disabled={isLoading}
      >
        {isLoading ? "Generating..." : "Generate Video"}
      </button>
      {isLoading && (
        <div className="mt-4">
          <div className="animate-spin rounded-full h-8 w-8 border-t-2 border-blue-500"></div>
        </div>
      )}
      {video && (
        <div className="mt-4">
          <video controls className="max-w-full">
            <source src={video} type="video/mp4" />
          </video>
        </div>
      )}
    </div>
  );
}
```

This code includes the frontend interface to interact with our AI video generator. we've added an **`<input>`** element for capturing the prompt from the user. When the user enters the prompt in the input box and clicks the "Generate Video" button, the prompt is sent to the **`/api/video-generator`** endpoint as a part of the request body. This ensures that the AI animated video generated is based on the user's provided prompt.

Here are few example prompts:

1. A cyberpunk styled city with shrooms and lavendar colors 
2. a journey, trending on Artstation
3. a beautiful forest by Asher Brown Duran, trending on Artstation
4. a beautiful portrait of a woman by Artgerm, trending on Artstation

## Conclusion

Lights, AI, Action! With Next.js and Replicate's **deforum_stable_diffusion** model, we've become animation wizards. Enter prompts, hit "Generate Video," and voilà, captivating short videos come to life.

Get ready to amaze your audience with AI-infused creativity. It's showtime for our AI animated video generator. Bravo, directors of the AI show!
