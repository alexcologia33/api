require 'rubygems'
require 'bundler'
Bundler.require

StandaloneMigrations::Tasks.load_tasks


begin
  require 'rspec/core/rake_take'
  RSpec::Core::RakeTask.new(:spec)
  task default: :spec
rescue LoadError
end


namespace :data do
  task :enviroment do
    ENV['USE_PARSER'] = 'true'
    require './config/boot'
  end

  desc 'get cota data'
  task cotas: :enviroment do
    CotaCrawlerService.save_from_cota_xml_parser
  end

  desc 'get deputados'
  task deputados: :enviroment do
    puts "\e[32mStarting deputados data... \e[0m"
    DeputadoCrawlerService.save_from_pesquisa_parser
    DeputadoCrawlerService.save_from_deputado_xml_parser
    DeputadoCrawlerService.save_from_deputado_about_parser
    DeputadoImageService.save_images_from_deputies_json_parser
  end

  task image: :enviroment do
    DeputadoImageService.save_images_from_deputies_json_parser
  end

  desc 'get video data'
  task video: :enviroment do
    DeputadoVideoService.save_video_from_deputados_parser
  end

  desc 'get proposition data'
  task proposition: :enviroment do
    puts "\e[32mStarting propositions data... \e[0m"
    DeputadoPropositionsService.save_propositions
  end

  desc 'run all data tasks'
  task all: ['db:create', 'db:migrate', :deputados, :cotas, :video, :proposition]
end
