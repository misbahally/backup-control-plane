# TODO

## Upgrade boto3/botocore safely

- [ ] Choose a boto3 release whose resolved botocore version includes the fix for
      [botocore#3755](https://github.com/boto/botocore/pull/3784) (botocore 1.43.77
      or later); keep boto3/botocore versions compatible.
- [ ] Update `apps/worker/pyproject.toml` and regenerate/commit
      `apps/worker/poetry.lock`. Ensure the worker Docker build installs from the
      committed lockfile instead of resolving dependencies with `poetry lock`.
- [ ] Review boto3's checksum defaults for compatibility with Wasabi and any
      other S3-compatible endpoints. If needed, set
      `request_checksum_calculation="when_required"` and
      `response_checksum_validation="when_required"` in the S3 client config.
- [ ] Test uploads against Wasabi: empty and non-empty objects, single-part and
      multipart transfers (including multipart copy), and configured encryption
      modes. Confirm `ETag` responses are preserved and the urllib3 header parsing
      warning is gone.
- [ ] Run the worker test suite, including
      `test_worker_s3_clients_do_not_send_expect_continue`; once the upgraded
      dependency is verified to fix the issue, consider removing the
      `Expect: 100-continue` workaround and adjust the regression test accordingly.
- [ ] Build and deploy the worker image, then verify a real backup run completes
      successfully against the configured S3-compatible storage.
