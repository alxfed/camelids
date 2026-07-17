Camelids are large herbivorous mammals in the family Camelidae.
<pre>
  pip install camelids
</pre>

Then:

```Python
from yaml import safe_load as yl
from camelids.messages import message

kwargs = """  # this is a string in YAML format
  max_tokens:   32000
  stop_sequences:
    - STOP
    - "\nTitle"
  temperature:  1.0
  top_k:        10
  top_p:        0.5
  thinking:     
    type: enabled
    budget_tokens: 24576
    display: summarized
  tool_choice:
    type: auto
    disable_parallel_tool_use: false
"""

instruction = 'You are a helpful assistant. Important: Do not use markdown or lists in your responses.'

get_weather_tool_str = """ # messages specific definition.
  name: get_weather
  description: Determine weather in my location
  input_schema:
    type: object
    properties:
      location:
        type: string
        description: The city and state
    additionalProperties: true
    required:
      - location
"""
tools = [yl(get_weather_tool_str)]

msg = [{'role': 'user', 'content': 'What is the weather like in Chicago, IL and Paris, France?'}]

thoughts, text = message(
    messages=msg,
    instructions=instruction,
    tools=tools,
    **yl(kwargs)
)
```
