import os
from pydantic import BaseModel, Field
from typing import List
from groq import Groq
import instructor

class TopicDetails(BaseModel):
    title: str
    details: List[str] = Field(..., description="A list of details about the topic")

def get_topic_input():
    return input("Enter a subject to explore (or 'exit' to quit): ")

def fetch_info(subject):
    api_client = Groq(
        api_key=os.environ.get(
            os.environ.get("GROQ_API_KEY"),
        ),
    )

    api_client = instructor.from_groq(api_client, mode=instructor.Mode.TOOLS)

    response = api_client.chat.completions.create(
        model="mixtral-8x7b-32768",
        messages=[
            {
                "role": "user",
                "content": f"Provide details about {subject}",
            }
        ],
        response_model=TopicDetails,
    )
    print(response.model_dump_json(indent=2))

if __name__ == "__main__":
    while True:
        user_input = get_topic_input()
        if user_input.lower() == 'exit':
            break
        fetch_info(user_input)
