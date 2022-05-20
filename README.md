## Rubocop --config + --regenerate-todo issue demo

It seems that `rubocop` doesn't fully consider the config file specified on the CLI via `--config` when regenerating the TODO file via `--regenerate-todo` when `.rubocop.yml` also exists. 

This repo contains a `.rubocop_todo.yml` generated **when .rubocop.yml looked like .rubocop_wip.yml**.

In other words, `.rubocop.yml` enabled all default cops when `.rubocop_todo.yml` was generated.

Now we've moved `.rubocop.yml` to `.rubocop_wip.yml` and created a new `.rubocop.yml` that disables all default cops and enables exactly one cop.

If I regenerate the TODO using the `.rubocop_todo.yml` config, I'd expect there to be no material changes to `.rubocop_todo.yml`.

However, it seems that rubocop will generate the `.rubocop_todo.yml` based on `.rubocop.yml` (i.e. it'll only contain the one enabled cop), despite being passed `.rubocop_wip.yml` via the `--config` switch, so long as `.rubocop.yml` exists.

### Reproduction

**Step to reproduce:**

Regenerate the TODO using the `.rubocop_wip.yml` config:
  ```bash
  $ rubocop --config .rubocop_wip.yml --regenerate-todo demo.rb
  ```

**Expected outcome:**

`.rubocop_todo.yml` looks generally the same, with `demo.rb` excluded from the 3x cops it contains offenses for.

**Actual outcome:**

`.rubocop_todo.yml` contains an exclusion for exactly one cop: the one enabled in `.rubocop.yml`.

### Notes

If we delete `.rubocop.yml` and run the command again, it works as expected.
