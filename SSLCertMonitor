#!/bin/bash

send_email() {
    # E-posta başlık ve içeriği
    current_date=$(date +"%Y-%m-%d")
    email_subject="SSL Kontrol - $current_date - $domain"
    email_content="Merhaba,\n\n$mail_body\n\nSaygılarımla"

    # E-posta başlık ve içeriğini birleştir
    email_header="Subject: $email_subject\nFrom: $from_email\nTo: $email_recipient\n"

    # E-postayı gönder
    echo -e "$email_header\n$email_content" | /usr/sbin/sendmail -f "$from_email" "$email_recipient"
}

# Alan adlarını bir diziye tanımla
domains=("domain1.com" "domain2.com")

# Birden fazla alıcı tanımla
email_recipients=("admin@enessahins.com.tr")			# change this
#email_recipients=("admin@enessahins.com.tr")

# Her bir alan adı için işlem yap
for domain in "${domains[@]}"; do
    # JSON verilerini çek
    json_data=$(curl -s "https://ssl-checker.io/api/v1/check/$domain")

    # JSON verilerini kontrol et
    if [ -z "$json_data" ]; then
        echo "Hata: $domain için JSON verisi alınamadı."
        continue
    fi

    days_left=$(echo "$json_data" | jq -r '.result.days_left')

    # Eğer days_left değeri boş değilse ve 15 gün veya daha az ise, işlem yap
    if [[ "$days_left" != "null" && $days_left -le 15 && $days_left -gt 0 ]]; then


        # JSON dosyasındaki verileri çıkar ve değişkenlere ata
        host=$(echo "$json_data" | jq -r '.host')
        issuer_o=$(echo "$json_data" | jq -r '.result.issuer_o')
        valid_from=$(echo "$json_data" | jq -r '.result.valid_from')
        valid_till=$(echo "$json_data" | jq -r '.result.valid_till')
        cert_exp=$(echo "$json_data" | jq -r '.result.cert_exp')
        cert_valid=$(echo "$json_data" | jq -r '.result.cert_valid')

        # Değişkenleri yazdır
        from_email="admin@enessahins.com.tr"
        mail_body="Host: $host\nİmza Sahibi Kuruluş: $issuer_o\nSona Erme Günü: $days_left\nBaşlangıç: $valid_from\Bitiş: $valid_till\nSertifika Süresi Dolmuş: $cert_exp\nSertifika Geçerli: $cert_valid"
        echo -e "$mail_body"    

        # Her bir alıcıya e-posta gönder
        for email_recipient in "${email_recipients[@]}"; do
            send_email
        done
    fi
done
