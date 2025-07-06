# Leetcode-283-Move-Zeroes


# 🧠 LeetCode Solutions by Witts

This repo contains my clean and well-documented solutions to selected **LeetCode problems**, primarily using **Python**.  
Each problem is organized by its ID and includes an explanation, solution code, and complexity analysis.


## 🧰 Languages & Tools

- Python 3.0
- Markdown
- Git / GitHub


class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Moves all 0s to the end of the array in-place while preserving the order of non-zero elements.
        Time Complexity: O(n)
        Space Complexity: O(1)
        """
        firstzero = 0
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[i], nums[firstzero] = nums[firstzero], nums[i]
                firstzero += 1

from collections.abc import AsyncGenerator, Iterable
from pydantic import BaseModel
from ragbits.chat.api import RagbitsAPI
from ragbits.chat.interface import ChatInterface
from ragbits.chat.interface.types import ChatContext, ChatResponse
from ragbits.core.embeddings import LiteLLMEmbedder
from ragbits.core.llms import LiteLLM
from ragbits.core.prompt import Prompt, ChatFormat
from ragbits.core.vector_stores import InMemoryVectorStore
from ragbits.document_search import DocumentSearch
from ragbits.document_search.documents.element import Element

class QuestionAnswerPromptInput(BaseModel):
    question: str
    context: Iterable[Element]

class QuestionAnswerPrompt(Prompt[QuestionAnswerPromptInput, str]):
    system_prompt = """
    You are a question answering agent. Answer the question that will be provided using context.
    If in the given context there is not enough information refuse to answer.
    """
    user_prompt = """
    Question: {{ question }}
    Context: {% for chunk in context %}{{ chunk.text_representation }}{%- endfor %}
    """

class MyChat(ChatInterface):
    async def setup(self) -> None:
        self.llm = LiteLLM(model_name="gpt-4.1-nano")
        self.embedder = LiteLLMEmbedder(model_name="text-embedding-3-small")
        self.vector_store = InMemoryVectorStore(embedder=self.embedder)
        self.document_search = DocumentSearch(vector_store=self.vector_store)
        await self.document_search.ingest("web://https://arxiv.org/pdf/1706.03762")

    async def chat(
        self,
        message: str,
        history: ChatFormat | None = None,
        context: ChatContext | None = None,
    ) -> AsyncGenerator[ChatResponse, None]:
        chunks = await self.document_search.search(message)
        prompt = QuestionAnswerPrompt(QuestionAnswerPromptInput(question=message, context=chunks))
        async for text in self.llm.generate_streaming(prompt):
            yield self.create_text_response(text)

if __name__ == "__main__":
    api = RagbitsAPI(MyChat)
    api.run()
