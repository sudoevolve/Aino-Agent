## Features

- Desktop AI Agent client interface  
- WebSocket streaming AI communication  
- Simple Tool system to execute local commands (browser, shell, etc.)  
- Markdown message rendering  
- Session and message history management  

---

This project implements an AI Desktop Agent built with Qt + QML + C++.
The core idea is to expose computer capabilities as Tools that the AI can call through natural language.

Instead of hard-coding commands, the system uses a Tool abstraction layer.
Each capability (system control, application control, terminal execution, etc.) is implemented as a tool.
The AI analyzes the user request and decides which tool to call.

Example workflow:

User Input
    ↓
AI Understanding
    ↓
Tool Call JSON
    ↓
ToolManager
    ↓
Execute Tool
    ↓
Return Result

Example tool call:

{
  "tool": "shutdown",
  "args": {
    "delay": 60
  }
}

---

Tool Categories

The agent tools are divided into four major categories.

1 System Tools
2 Application Tools
3 Terminal & File Tools
4 Advanced Agent Tools

Each category represents a different layer of computer capability.

---

1 System Tools

System tools interact directly with the operating system to control or query system state.

These tools provide the agent with basic system management abilities.

Core Tools

shutdown
restart
sleep
lock_screen
set_volume
set_brightness

System Information

get_system_info
get_cpu_usage
get_memory_usage
get_disk_usage
get_network_status

Example

User request:

How much memory is my computer using?

Tool call:

{
  "tool": "get_memory_usage"
}

---

2 Application Tools

Application tools allow the agent to control installed applications.

The AI can launch software, close it, or interact with it.

Core Tools

open_app
close_app
switch_app
list_running_apps

Example

User request:

Open Chrome

Tool call:

{
  "tool": "open_app",
  "args": {
    "name": "chrome"
  }
}

Another example:

Open CapCut and load my last project

Possible execution flow:

open_app(capcut)
load_project(last_project)

---

3 Terminal & File Tools

Terminal tools allow the AI to execute commands and perform file operations.

This category is extremely powerful because it exposes full system capabilities.

File Operations

read_file
write_file
delete_file
copy_file
move_file
create_folder
list_directory
search_file
file_info

Terminal Execution

run_command
get_command_output

Example

User request:

Delete temporary files in downloads

Tool execution plan:

list_directory(downloads)
analyze_files
delete_file(tmp_files)

Another example:

Check git status

Tool call:

{
  "tool": "run_command",
  "args": {
    "command": "git status"
  }
}

---

4 Advanced Agent Tools

Advanced tools give the agent perception and reasoning abilities.

These tools allow the AI to interact with the visual desktop environment.

Screen Understanding

screenshot
analyze_screen
detect_ui_elements

The agent can capture the screen and send the image to a vision model for analysis.

Example:

User: Why is my code not compiling?

Execution flow:

screenshot
analyze_screen
explain_error

---

Desktop Interaction

move_mouse
click_mouse
type_text
press_key

This allows the agent to operate software like a human user.

Example:

User: Export the video in CapCut

Execution plan:

detect_ui_elements
click(export_button)
click(confirm_button)

---

Tool Architecture

Each tool implements a common interface.

Example structure:

agent
 ├── tool
 │   ├── system
 │   │    shutdown_tool
 │   │    system_info_tool
 │   │
 │   ├── app
 │   │    open_app_tool
 │   │    close_app_tool
 │   │
 │   ├── terminal
 │   │    run_command_tool
 │   │
 │   └── file
 │        read_file_tool
 │        delete_file_tool

All tools are registered in a ToolManager.

AI
 ↓
ToolManager
 ↓
Tool Execution

---

Example Agent Scenario

User request:

Clean my downloads folder

Execution plan:

list_directory(downloads)
identify_old_files
delete_file(old_files)

Another example:

Open Chrome and search Qt QML tutorial

Execution plan:

open_app(chrome)
open_url(search_url)

---

Future Extensions

The tool system can be extended with additional capabilities.

Possible future tools:

browser automation
clipboard control
task scheduler
calendar integration
notification system
code analysis tools

Because the system uses a tool abstraction layer, new capabilities can be added easily without changing the core agent architecture.

---

Summary

This project builds an AI Desktop Agent that converts natural language into executable system actions.

Key features:

AI chat interface
Tool-based architecture
Natural language computer control
Cross-platform desktop UI
Extensible agent tool system

By abstracting system capabilities into tools, the agent becomes a flexible platform for intelligent human-computer interaction.