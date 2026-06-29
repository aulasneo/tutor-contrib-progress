# Progress plugin for Tutor

Progress plugin for [Tutor](https://docs.tutor.edly.io).

This plugin installs [aulasneo/openedx-progress](https://github.com/aulasneo/openedx-progress) into the Open edX image. See the [openedx-progress README](https://github.com/aulasneo/openedx-progress#readme) for more details about the Open edX Django app, its models, and management commands.

## Installation

```bash
pip install git+https://github.com/aulasneo/tutor-contrib-progress
```

## Usage

Enable the plugin:

```bash
tutor plugins enable progress
```

After enabling the plugin, rebuild the Open edX image:

```bash
tutor images build openedx
```

If you use a private registry, push the rebuilt image:

```bash
tutor images push openedx
```

Restart containers for local deployments:

```bash
tutor local restart
```

Restart pods for Kubernetes deployments:

```bash
tutor k8s start
```

Then run the init command:

```bash
tutor {local|k8s} do init --limit progress
```

This plugin adds the following manual job:

```bash
tutor {local|k8s} do backfill-course-completion-summaries
```

Useful options:

- `--course-id course-v1:edX+DemoX+Demo_Course` limit to a course id.
- `--user-id 123` can be repeated to process specific learners.
- `--batch-size 500` controls database iteration batches.
- `--sleep 0.5` pauses between batches.
- `--dry-run` selects learners without writing rows.
- `--force` recomputes rows that already exist.

For Kubernetes users, this plugin sets up a CronJob to process dirty course completion summaries every hour at a randomized minute.

For local users, set a cronjob to run `tutor local exec lms ./manage.py lms process_dirty_course_completion_summaries` periodically.

## Configuration

Note: Only relevant for K8s deployments.

- `PROGRESS_COMPLETION_SUMMARIES_SCHEDULE`: Set the schedule for processing dirty course completion summaries. Default is `"<random-minute> * * * *"` (every hour, with a randomized minute).

## License

This software is licensed under the terms of the AGPLv3.

## Disclaimer

Part of this code was developed with the aid of AI tools.
