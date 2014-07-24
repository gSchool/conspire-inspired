CreateEmail = ->(sender, recipient, days_ago, in_reply_to = nil) {
  id = SecureRandom.uuid
  message = Mail.new do
    in_reply_to in_reply_to if in_reply_to
    to recipient
    from sender
    date Date.today - days_ago
    subject Faker::Lorem.sentence
    body Faker::Lorem.paragraphs.join("\n\n")
    message_id "<#{id}@example.com>"
  end
  File.open("data/#{id}.eml", "w") { |f| f.puts message.to_s }
  message.message_id
}

MakeCurrentFriends = ->(sender, recipient) {
  original = CreateEmail.(sender, recipient, 7*6)
  CreateEmail.(recipient, sender, 7*5, original)

  CreateEmail.(sender, recipient, 7*4)

  original = CreateEmail.(sender, recipient, 7*3)
  CreateEmail.(recipient, sender, 12, original)
}

MakeOldFriends = ->(sender, recipient) {
  [18, 20, 22, 24, 26, 28, 30, 32].each do |weeks_ago|
    original = CreateEmail.(sender, recipient, 7*weeks_ago)
    CreateEmail.(recipient, sender, 7*(weeks_ago - 1), original)
  end

  [15, 17].each do |weeks_ago|
    CreateEmail.(sender, recipient, 7*weeks_ago)
  end

  [16, 18].each do |weeks_ago|
    CreateEmail.(sender, recipient, 7*weeks_ago)
  end
}

desc "Generate emails with recent time stamps"
task :generate do
  require 'mail'
  require 'faker'
  require 'securerandom'
  I18n.enforce_available_locales = false

  rm_r Dir.glob("data/*.eml")

  MakeCurrentFriends.call("alice@example.com", "ephram@example.com")
  MakeOldFriends.call("bob@example.com", "hillary@example.com")
  MakeOldFriends.call("cecelia@example.com", "fatima@example.com")
  MakeCurrentFriends.call("inez@example.com", "joe@example.com")
end
