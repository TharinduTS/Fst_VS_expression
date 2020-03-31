# Fst_VS_expression
# R plotting all chrs in one
```r

all_populations<-c("wild_all")

#**********************************
for (current_pop in all_populations) {
  
  
  #population
  pop<-current_pop
  
  #**********************************
  
  setwd(paste("C:/Users/Tharindu/OneDrive - McMaster University/for lab and research/Tharindu on Mac/lab/expression_data/filtered_again_fst/",pop,sep = ''))
  
  
  library(ggplot2)
  plot_list<-list()
  library(pracma)
  x<-0
  
  #remove scientific notation
  options(scipen=999)
  
  #for other chrs
  ch_numbers<-c("Chr1","Chr2","Chr4","Chr5","Chr6","Chr8","Chr9","Chr10")
  
  
  
  
  for (i in ch_numbers) {
    chrom_data<-read.table(paste("Fst_Trop_",pop,"_F_Vs_M_",i,".windowed.weir.fst",sep=''),header = T)
    attach(chrom_data)
    #chrom_data
    
    exp_data<-read.csv(paste("./../exp_for_chrs/",i,".csv",sep=''),header = T)
    
    chrom_and_exp_data<-merge(chrom_data,exp_data,by=c("CHROM","BIN_START"))
      
    #mv_avg<-movavg(MEAN_FST,5,type = "s")
    #chrom_data$mv_avg=mv_avg
    
    x<-x+1
    
    special_plot<-ggplot(chrom_and_exp_data,aes(x=MEAN_FST,y=mean.abs.logFC))+
      geom_point(color="black",size=0.5)+
      theme_bw()+
      labs(title = paste("Mean_Fst_",current_pop),subtitle = "Blue=Chr7           Red=Chr3")
    
  }
  
  
  #for second special chr
  
  second_special_chr<-"Chr3"
  second_special_chr_data<-read.table(paste("Fst_Trop_",pop,"_F_Vs_M_",second_special_chr,".windowed.weir.fst",sep=''),header = T)
  second_special_exp_data<-read.csv(paste("./../exp_for_chrs/",second_special_chr,".csv",sep=''),header = T)
  second_special_chrom_and_exp_data<-merge(second_special_chr_data,second_special_exp_data,by=c("CHROM","BIN_START"))
  
  special_plot<-special_plot+
    geom_point(data = second_special_chrom_and_exp_data, mapping = 
                 aes(x=MEAN_FST,y=mean.abs.logFC),size=1,color="red",alpha=0.75)
  
  #for first special chr
  
  special_chr<-"Chr7"
  special_chr_data<-read.table(paste("Fst_Trop_",pop,"_F_Vs_M_",special_chr,".windowed.weir.fst",sep=''),header = T)
  special_exp_data<-read.csv(paste("./../exp_for_chrs/",special_chr,".csv",sep=''),header = T)
  special_chrom_and_exp_data<-merge(special_chr_data,special_exp_data,by=c("CHROM","BIN_START"))
  
  special_plot<-special_plot+
    geom_point(data =special_chrom_and_exp_data, mapping = 
                 aes(x=MEAN_FST,y=mean.abs.logFC),color="blue",alpha=0.75,size=1)
  print("done")
  
  
  
  #ggsave(paste("~/Desktop/OneDrive - McMaster University/Tharindu on Mac/lab/Pi/step50000/Borealis/Different_populations/",pop,"/",pop,"_PI_mean_substracted.pdf",sep = ''),plot = Final_plot_grid,width = 15,height = 30)
  #ggsave(paste("~/Desktop/OneDrive - McMaster University/Tharindu on Mac/lab/Pi/step50000/Borealis/Different_populations/all_populations_PI-mean/",pop,"_PI_mean_substracted.pdf",sep = ''),plot = Final_plot_grid,width = 15,height = 30)
  ggsave(paste("C:/Users/Tharindu/OneDrive - McMaster University/for lab and research/Tharindu on Mac/lab/expression_data/filtered_again_fst/out/",pop,"_Mean_Fst_VS_exp_all_in_one.pdf",sep = ''),plot = special_plot,width = 4,height = 4,bg="transparent")
  #*************************************
  
}
```
