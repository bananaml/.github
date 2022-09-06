# Dev vs Prod Flags
At root of go microservice define an env.go file with an init() function

This init function looks for a single required variable "ENV" and then sets either prod or dev env vars accordingly

This makes it easy to switch between dev and prod, keep track of env var changes, and works with any dev flow

your init() function should handle 3 cases as follows
```
func init() 
    if os.Getenv("ENV) == "prod" {
        // set environment variables via either secret manager or hard coded here
        os.Setenv("some var", "some value)...
        thirdparty.secretmanager.getVar("some secret var", "some secret value")

        // All env vars should be accessible via os.getenv("var name")
    } else if os.Getenv("ENV") == "dev" {
        // same deal as above, except use dev values for staging databases etc
    } else {
        // enforce ENV is set and valid or else quit
		panic("ENV must be set to prod or dev")
    }
}
```

Then access them in your program via os.Getenv("your var...")
