# Tududi tags

Before writing, read the available tag catalog and the target task's assigned
tags. Choose no more than three additions that describe durable facets of the
task, such as its project, domain, or work type. The limit applies to additions
for this operation, not the task's final total. Respect the connected server's
maximum total and choose fewer additions when the task is near that limit.

Reuse an existing semantic match before adding one. Compare normalized case,
spacing, punctuation, singular/plural forms, and established naming patterns;
do not create a synonym merely to prefer different wording. When no relevant
match exists, include the new name in the task operation; Tududi creates it
when assigning the task's tags.

`update_task.tags` replaces the collection. Preserve all existing tag names,
add the selected names, deduplicate case-insensitively, and submit the complete
intended set. Do not call `create_tag` separately. Re-read the task after the
update. On an uncertain response, inspect the saved task before retrying so a
task or tag is not duplicated.
