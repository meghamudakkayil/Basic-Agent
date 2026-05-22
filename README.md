# Basic-Agent
To get started, I build an agent that answer anything you ask it.
import os
from anthropic import Anthropic

client = Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

SYSTEM_PROMPT = """You are a helpful, knowledgeable general-purpose agent.
Answer questions clearly and concisely. Be friendly and direct."""

def chat(history: list, user_message: str) -> str:
    history.append({"role": "user", "content": user_message})

    response = client.messages.create(
        model="claude-sonnet-4-5",
        max_tokens=1024,
        system=SYSTEM_PROMPT,
        messages=history,
    )

    reply = response.content[0].text
    history.append({"role": "assistant", "content": reply})
    return reply


def main():
    history = []
    print("Agent ready. Type 'exit' to quit.\n")

    while True:
        user_input = input("You: ").strip()

        if not user_input:
            continue
        if user_input.lower() in ("exit", "quit"):
            print("Goodbye!")
            break

        reply = chat(history, user_input)
        print(f"\nAgent: {reply}\n")


if __name__ == "__main__":
    main()
