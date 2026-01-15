I am in a workshop "how to build an agent" so I don't have much time, build me an agent in golang.

## Important
- **Critical**: The Antrophic API Key is available in this environment variable: `MY_AGENT_API_KEY`
- In scope are three file tools: `list_files`, `read_file`, `edit_file` and the `bash` tool
- Use a files and folders to seperate the code so it's easier to read.
- Libs to use: Cobra, lipgloss, Antropic SDK
- create Makefile to build the agent
- Don't modify the readme
- You test the agent before you are done

## Interface

There are two interfaces:
- An interactive "chatlike" interface with a snazzy lipglos interface. Do your thing be creative!
- one shot cli usage: `echo "<prompt>" | ./agent`

### Interactive Interface

- Important: Tool uses are shown in the interface so the user sees what is happening.
- CTRL+c or CTRL+d quit the agent
- esc stops interrupts whatever the agent is doing and the user can prompt again.

## Conversation log

The agent creates a log `conversaion.log`.
It is Human readable but "nearly wireformat".

### Examples

**User message to API**:
```
[2026-01-14 10:30:45] MESSAGE TO API
ROLE: user
CONTENT:
  [0] TEXT:
    List all markdown files
```

**API response with tool call**:
```
[2026-01-14 10:30:52] MESSAGE FROM API
MESSAGE_ID: msg_01ABC123xyz
MODEL: claude-sonnet-4-5-20250929
ROLE: assistant
STOP_REASON: tool_use
USAGE:
  INPUT_TOKENS: 782
  OUTPUT_TOKENS: 57
CONTENT:
  [0] TEXT:
    I'll list the markdown files.
  [1] TOOL_USE:
    ID: toolu_01ABC
    NAME: list_files
    TYPE: tool_use
    INPUT:
      path: "."
```

**Tool result to API**:
```
[2026-01-14 10:30:52] MESSAGE TO API
ROLE: user
CONTENT:
  [0] TOOL_RESULT:
    TOOL_USE_ID: toolu_01ABC
    IS_ERROR: false
    CONTENT:
      Files in .:
      - README.md (file, 1234 bytes)
```

## Testing

You can write unit tests where you find it applicable.

But the most important test are the functional test you are doing via bash:

```
echo "Which markdown files are present here?" | ./agent
There are two markdown files: PROMPT.md, README.md
```

Verify via one shot usage:
- the agent works and corretly with the Antropic SDK
- all tool calls work
  - list_files
  - read_files
  - edit_file
  - bash
- the conversation.log is correctly written

Verify the Interactive mode quits with the correct key combinations. We don't want to trap the user.
