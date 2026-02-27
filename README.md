# TDMatrixBot

A python module to make bots for Matrix homeserver's.

## Example

```python
import asyncio
from pathlib import Path
from typing import Dict
from tdmatrixbot.bot import MatrixBot
from tdmatrixbot.types import MatrixRoom

bot = MatrixBot(
    host="matrix.example.com",
    token="your_matrix_homeserver_compatibility_token",
    data_dir=Path("/data"),
)


@bot.command("help", help_description="List all available bot commands.")
async def help(room: MatrixRoom, event: Dict, cmd_value: str):
    commands = list(bot._commands.values())
    plain = "".join(f"- {cmd.name} - {cmd.help_description}\n" for cmd in commands)
    html = (
        "<ul>"
        + "".join(
            f"<li><code>!{cmd.name}</code> - {cmd.help_description}</li>"
            for cmd in commands
        )
        + "</ul>"
    )
    await bot.homeserver.send_message(
        room.id,
        f"Commands:\n\n{plain}",
        f'<p><span data-mx-color="#00FFFF">Available commands:</span></p>{html}',
    )


if __name__ == "__main__":
    asyncio.run(bot.run())
```