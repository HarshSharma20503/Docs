# Gemini API Snippet

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";
import dotenv from "dotenv";

dotenv.config();

const GetGiminiResponse = async (prompt) => {
    try {
        const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);

        const generationConfig = {
            maxOutputTokens: 250,
            temperature: 0, //this is used to control the randomness of the output, it ranges from 0 to 1, the higher the value the more random the output
            topP: 1,
            topK: 16, //this is used to control the diversity of the output, it ranges from 1 to infinity, the higher the value the more diverse the output
        };

        const model = genAI.getGenerativeModel({
            model: "gemini-pro",
            generationConfig,
        });

        const result = await model.generateContent(prompt);
        const response = result.response;
        const text = response.text();
        return text;
    } catch (e) {
        console.log(e);
        return "Error Generating AI response. Please try again later.";
    }
};

const Prompt = "Write a program to print hello world in C++";
const GeminiResponse = await GetGiminiResponse(Prompt);
console.log(GeminiResponse);
```
