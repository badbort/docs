## Taint

Example of tainting an instance causing the next plan/apply to recreate it

```powershell
terraform taint 'null_resource.codacy_quality_settings[\"codacy-test-pull-requests\"]'
```
