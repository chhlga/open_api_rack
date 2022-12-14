## Installation

Add this line to your application's Gemfile:

```ruby

gem 'open_api_rack'

```

And then execute:

    $ bundle install

## Usage

1) It's pretty simple. First register middleware for a test environment  

``` config/enviroments/test.rb

Rails.application.configure
  config.middleware.insert_before 0, OpenApiRack::Middleware
end

```

Please, keep it only for test env!

2) Then, you need to configure the gem. Create an initializer file

```config/initializers/open_api_rack.rb

OpenApiRack.configure do |config|
  # list of headers you want to follow
  config.headers_list = %w(access_token client uid access-token)

  # by default gem is enabled
  config.disable_by_default = true
end

```

`header_list` setting defines the headers you want to follow

3) Final step, start writing your request specs

``` spec/requests/users_spec.rb

RSpec.describe "Api::V1::Users", type: :request do
  describe "GET /" do
    before(:each) do
      get "/api/v1/users", params: { per_page: 5, page: 1, sort: 'email', order: 'desc' }
    end

    it "returns http success" do
      expect(response).to have_http_status(:success)
    end
  end
end
```

You may also exclude specific request from documentation by adding `{ "OA_SKIP_EXAMPLE" => "true" }` to your request headers  

``` spec/requests/users_spec.rb
get "/api/v1/users", params: { per_page: 5, page: 1, sort: 'email', order: 'desc' }, headers: { "OA_SKIP_EXAMPLE" => "true" }
```

If you decide to disable doc generation, you can turn it on for specific request
``` spec/requests/users_spec.rb
get "/api/v1/users", params: { per_page: 5, page: 1, sort: 'email', order: 'desc' }, headers: { "OA_INCLUDE_EXAMPLE" => "true" }
```

Generated docs stored in /public/open-api.yaml

## Contributing

Gem in active development

Known issues:

- not working for DELETE HTTP method

- not working for :no_content response

- can not define fields types


Bug reports and pull requests are welcome on GitHub at https://github.com/chhlga/open_api_rack. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [code of conduct](https://github.com/[USERNAME]/open_api_rack/blob/main/CODE_OF_CONDUCT.md).

## License
The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct
Everyone interacting in the OpenApiRack project's codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/[USERNAME]/open_api_rack/blob/main/CODE_OF_CONDUCT.md).

