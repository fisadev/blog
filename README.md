# Install env

    uv sync


# Posting an entry

Create the entry in `/posts/`, then add and commit it to master, and then run (from master):

    uv run nikola github_deploy


# Building and testing locally

    uv run nikola build
    uv run nikola serve
