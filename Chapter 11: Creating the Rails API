Creating the Rails API
==========

我们需要一个和Ember沟通的Rails API,以便于保存和检索数据.

**The Active Model Adapter**

当我们使用Ember Rails生成器命令设置Ember时,它添加了一个叫做Active Model Adapter的东东.如果你打开store.js就会看到它:

    // app/assets/javascripts/store.js
    App.Store = DS.Store.extend({});
    App.ApplicationAdapter = DS.ActiveModelAdapter.extend({});
    
如果你没有看到这段代码,那就用它替换掉你的store.js中的内容.

Active Model Adapter可以让Ember通过[Active Model Serializers][1]与Rails后端沟通,通常你需要引入`active_model_serializers` gem,但是Ember Rails已经有它作为一个依赖项.

**命名空间API请求**

我们需要告诉Ember使用`api/v1/`预设所有的API请求,我们会对api进行版本控制,将这两行代码添加到store.js的顶部:

    // app/assets/javascripts/store.js
    DS.RESTAdapter.reopen({
      namespace: 'api/v1'
    })
    
**在Rails中建模Leads**

首先创建我们的leads:

    rails g migration create_leads first_name:string last_name:string email:string phone:string status:string notes:text
    
打开migration并且确保添加时间戳:

    class CreateLeads < ActiveRecord::Migration
      def change
        create_table :leads do |t|
          t.string :first_name
          t.string :last_name
          t.string :email
          t.string :phone
          t.string :status
          t.text :notes
    
          t.timestamps
        end
      end
    end
    
运行migration:

    rake db:migrate
    
现在我们创建Rails模型:

    # app/models/lead.rb
    class Lead < ActiveRecord::Base
    end
    
**The Rails Serializer**

添加序列化器,你需要列出想要序列化到JSON并且发给Ember的所有的特性(attribute):

    # app/serializers/lead_serializer.rb
    class LeadSerializer < ActiveModel::Serializer
      attributes :id, :first_name, :last_name, :email, :phone, :status, :notes
    end
    
**API控制器**

现在,我们有了我们的模型和序列化器,我们可以创建API控制器了.

首先我们需要API控制器的路由,把它们添加到Rails router的顶部:

    # config/routes.rb
    namespace :api do
      namespace :v1 do
        resources :leads
      end
    end
    
现在创建控制器,这里都是相当标准的行为:

    # app/controllers/api/v1/leads_controller.rb
    class Api::V1::LeadsController < ApplicationController
      respond_to :json
    
      def index
        respond_with Lead.all
      end
    
      def show
        respond_with lead
      end
    
      def create
        respond_with :api, :v1, Lead.create(lead_params)
      end
    
      def update
        respond_with lead.update(lead_params)
      end
    
      def destroy
        respond_with lead.destroy
      end
    
      private
    
      def lead
        Lead.find(params[:id])
      end
    
      def lead_params
        params.require(:lead).permit(:first_name, :last_name, :email, :phone, :status, :notes)
      end
    
    end
    
**See it in Action**

我们创建一些记录,看看我们的API实际的工作.

首先添加ffaker gem:

    # Gemfile
    gem 'ffaker'
    
然后创建一个populate任务:

    # lib/tasks/populate.rake
    namespace :db do
      task populate: :environment do
    
        Lead.destroy_all
    
        def random_status
          ['new', 'in progress', 'closed', 'bad'].sample
        end
    
        20.times do
          Lead.create(
            first_name: Faker::Name.first_name,
            last_name: Faker::Name.last_name,
            email: Faker::Internet.email,
            phone: Faker::PhoneNumber.phone_number,
            status: random_status,
            notes: Faker::HipsterIpsum.words(10).join(' ')
            )
        end
    
      end
    end

我们使用[Hipster Ipsum][2]做为备注,来增添乐趣.

运行populate任务:

    rake db:populate
    
重启服务器,现在你应该可以访问[http://localhost:3000/api/v1/leads.json][3] 并且看到所有leads的JSON.[http://localhost:3000/api/v1/leads/1.json][4]会显示第一个lead.

**Puma**

虽然我们使用Rails做一些微不足道的事情,但是我们要从Webrick切换到[Puma][5],它更快并且支持多线程,你必须要添加Puma到你的Gemfile:

    # Gemfile
    gem 'puma'
    
Bundler并且重启你的服务器.

这就是Rail方面的一切！现在,我们可以开始有趣的事情了.


  [1]: https://github.com/rails-api/active_model_serializers
  [2]: http://hipsum.co/
  [3]: http://localhost:3000/api/v1/leads.json
  [4]: http://localhost:3000/api/v1/leads/1.json
  [5]: http://puma.io/
