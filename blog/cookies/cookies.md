# HTTP cookies in Rails

## What cookies do

- Cookies are small key-value values stored by the browser.
- The browser sends matching cookies with future requests to the same domain/path.
- Rails exposes cookies through the `cookies` helper.

## Basic usage

```ruby
cookies[:theme] = "dark"
cookies[:theme] # "dark"
```

## Cookie options

```ruby
cookies[:locale] = {
  value: "en",
  expires: 1.year.from_now,
  httponly: true,
  secure: Rails.env.production?,
  same_site: :lax
}
```

## Signed cookies

- Prevent tampering.
- Useful when the value does not need to be secret but must be trusted.

```ruby
cookies.signed[:user_id] = current_user.id
cookies.signed[:user_id]
```

## Encrypted cookies

- Prevent reading and tampering.
- Useful for small sensitive values, though sessions are often better for login state.

```ruby
cookies.encrypted[:return_to] = request.fullpath
cookies.encrypted[:return_to]
```

## Permanent cookies

```ruby
cookies.permanent.signed[:remember_user_id] = current_user.id
```

## Deleting cookies

```ruby
cookies.delete(:theme)
```

## Security notes

- Use `httponly` to block JavaScript access where possible.
- Use `secure` in production so cookies are only sent over HTTPS.
- Use `same_site` to reduce cross-site request risks.
- Do not put secrets in plain cookies.

## Interview notes

- Sessions in Rails are often implemented with encrypted cookies.
- Signed means integrity; encrypted means confidentiality plus integrity.
- Cookies are sent on every matching request, so keep them small.
