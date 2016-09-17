--- 
layout: post
title:  一个客服端应用
category:  Ruby
tag: 思考
---
> 第一篇我们构建了Oauth 服务应用,现在我们创建一个发客服端应用。

###  omniauth-oauth2 gem,允许我们在试用Oauth2的时候，仅通过一个简单的配置文件(strategy)就可以办到。

```ruby
gem 'omniauth-oauth2'
``` 

### Omniauth 是一个通用gem,它允许程序员用它使用各种oath提供者，通过不同的配置。

### 在lib下创建doorkeeper.rb

```ruby
require 'omniauth-oauth2'

module OmniAuth
  module Strategies
    class Doorkeeper < OmniAuth::Strategies::OAuth2
      option :name, 'doorkeeper'
      option :client_options, {
        site:          'http://localhost:3000',
        authorize_url: 'http://localhost:3000/oauth/authorize'
      }

      uid {
        raw_info['id']
      }

      info do
        {
          email: raw_info['email'],
        }
      end

      extra do
        { raw_info: raw_info }
      end

      def raw_info
        @raw_info ||= access_token.get('/me').parsed
      end
    end
  end
end
``` 
