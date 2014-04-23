# PAM OTP session
This set of script allow PAM to remember that an user is already successfully authentified against a two (plain text) factor auth system. Useful when an app remembers the credential for the user, as browsers with basic auth or IMAP clients etc usualy do.

## Installation

Make sure you have the `pam_script.so` available on your system. You can get it on Debian-like-systems by installing the `libpam-script` package or by compiling it from this Github repo : [jeroennijhof/pam_script](https://github.com/jeroennijhof/pam_script)

Put the two scripts contained in `scripts` in a folder. Then point `pam_script.so` to it in your auth description in `/etc/pam.d/`, as `auth sufficient` then to `account sufficient`, as shown in the example : 

```bash
# first call the the scripts, set to 'auth sufficient'
auth	sufficient		pam_script.so dir=/path/to/pam_otp_session/

# begin normal two factor authentification (with Yubico OTP here)
auth    required		pam_yubico.so try_first_pass id=16 authfile=/path/to/authorized_yubikeys url=https://api.yubico.com/wsapi/2.0/verify?id=%d&otp=%s
#auth  	required		[put here further authentification]
#auth	required		(against database, unix password system, etc)
#auth	required		...
# ed normal two factor authentification

# second call if auth succeed, set to 'account sufficient' (sufficient or whatever)
account	sufficient		pam_script.so dir=/path/to/pam_otp_session/
```

And you're good.

## Problems ?

Open an issue !

Should be good using Google Auth and other OTP providers, otherwise, open an issue !

(Problems with Yubico PAM module ? [Yubico/yubico-pam](https://github.com/Yubico/yubico-pam))