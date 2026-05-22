# W6 Side Challenge Reflections

## Multi-account operation

To run `costctl` against 100 AWS accounts, I would add an account inventory,
assume a standard cross-account role in each account, and loop commands across
accounts and regions. Every output row should include account, region, resource
type, resource ID, and tags so the result can be aggregated into a CSV or report.

For write commands such as `terminate`, `clean --apply`, and `migrate-gp3`, I
would require explicit account and region selection plus a reviewed dry-run plan.
That gives an audit trail and avoids applying destructive actions account-wide by
accident.

## `idle` vs Trusted Advisor

The `idle` command is useful for fast checks on short-lived lab resources. A
24-hour CPU average can quickly find practice instances left running, and the
local tag filter lets me skip resources marked `keep=true`.

Trusted Advisor is better for production cleanup decisions because it uses a
longer observation window and is designed around AWS best-practice checks. I
would trust `idle` more for workshop cleanup, and Trusted Advisor more for
long-lived production systems where weekly workload patterns matter.

## `clean --apply` blast radius

If `clean --tag Environment=dev --apply` ran in a shared account, I would want a
dry-run plan that must be reviewed, a required owner/application tag in addition
to environment, an account allowlist, and a maximum deletion count. I would also
log every target resource ID before applying changes.

The current implementation keeps `clean` dry-run by default and only targets
non-terminal EC2 instances plus available EBS volumes. That is a good baseline,
but production use should add stronger confirmation and approval controls.

## AI assistance

AI assistance was used to implement and review the command stubs, then verify
the behavior against the provided tests. The parts that needed active review
were the AWS API choices, S3 tag merge behavior, the S3 non-empty delete guard,
and the safety contract around `clean` and `terminate`.

I would not treat AI-generated AWS automation as production-ready without tests
and dry-run evidence. The most important manual checks are whether each command
uses the right AWS API, whether destructive commands are guarded, and whether
the output is clear enough for review before action.

## W7 carry-over

I would keep `list`, `tag`, `cost`, `idle`, and `migrate-gp3` for W7 because
they support visibility, cleanup preparation, and cost optimization. I would keep
`terminate` and `clean` only after adding stronger account scoping, approval, and
logging because their blast radius is higher.
