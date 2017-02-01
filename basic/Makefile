include .source_env

check_env:
ifndef AWS_SECRET_KEY
	$(error AWS_SECRET_KEY is undefined)
endif
ifndef AWS_ACCESS_KEY
	$(error AWS_ACCESS_KEY is undefined)
endif

keygen:
	if [ ! -d "./ssh_keys" ]; then \
        /bin/mkdir ./ssh_keys; \
	fi
	if [ ! -f "./ssh_keys/rsa" ]; then \
		/usr/bin/ssh-keygen -b 2048 -t rsa -f ./ssh_keys/rsa -q -N ""; \
	fi

get:
	if [ -f ./terraform.tfstate ]; then \
		./terraform get --update; \
	fi

plan:
	./terraform plan -var aws_access_key=$(AWS_ACCESS_KEY) -var aws_secret_key=$(AWS_SECRET_KEY)

apply: check_env
	if [ ! -f ./terraform.tfstate ]; then \
	  ./terraform apply -no-color -parallelism=3 -var aws_access_key=$(AWS_ACCESS_KEY) -var aws_secret_key=$(AWS_SECRET_KEY)  ; \
		fi
	./terraform apply -no-color -parallelism=3 -var aws_access_key=$(AWS_ACCESS_KEY) -var aws_secret_key=$(AWS_SECRET_KEY)


destroy: check_env
	if [ ! -f ./terraform.tfstate ]; then \
	  ./terraform destroy -no-color -parallelism=3 -var aws_access_key=$(AWS_ACCESS_KEY) -var aws_secret_key=$(AWS_SECRET_KEY) ; \
		fi
	./terraform destroy -force -no-color -parallelism=3 -var aws_access_key=$(AWS_ACCESS_KEY) -var aws_secret_key=$(AWS_SECRET_KEY) ;
